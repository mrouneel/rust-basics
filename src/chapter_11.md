# Code for a webserver in Rust to connect it to a database i

```Rust
use std::net::SocketAddr;
use std::sync::OnceLock;

use axum::extract::Path;
use axum::response::IntoResponse;
use axum::Router;
use axum::routing::get;
use sqlx::{Executor, query, SqlitePool};
use sqlx::sqlite::SqlitePoolOptions;
use tokio::net::TcpListener;

static DB_POOL: OnceLock<SqlitePool> = OnceLock::new();

async fn get_pool() -> &'static SqlitePool {
    if let Some(x) = DB_POOL.get() {
        return x;
    }

    let pool = SqlitePoolOptions::new()
        .max_connections(100)
        .connect("sqlite:mydb.sqlite")
        .await
        .expect("Failed to connect to the database!");
    DB_POOL.get_or_init(move || pool)
}


async fn fruit(Path(fruit_name): Path<String>) -> impl IntoResponse {
    let connection = get_pool().await;
    let found = sqlx::query!("select name, price from fruits where name = ?", fruit_name)
        .fetch_optional(connection).await;

    match found {
        Ok(record) => {
            match record {
                None => {
                    format!("No fruit exists by the name '{fruit_name}'")
                }
                Some(record) => {
                    format!("The fruit '{fruit_name}' currently has a price of {}", record.price)
                }
            }
        }
        Err(e) => {
            format!("Failed to get price: {e}")
        }
    }
}


async fn create_fruit(Path((fruit_name, price)): Path<(String, u32)>) -> impl IntoResponse {
    let connection = get_pool().await;
    let result = query!("INSERT INTO fruits (name, price) VALUES (?, ?)", fruit_name, price)
        .execute(connection).await;


    match result {
        Ok(result) => {
            format!("Inserted fruit. {} row(s) affected", result.rows_affected())

        }
        Err(e) => {
            format!("Failed to insert fruit: {e}")
        }
    }
}


async fn list_fruits() -> impl IntoResponse {
    let connection = get_pool().await;
    let result = query!("SELECT name, price from fruits")
        .fetch_all(connection).await;


    match result {
        Ok(all_fruits) => {
            format!("Current Fruit: {:#?}", all_fruits)
        }
        Err(e) => {
            format!("Failed to insert fruit: {e}")
        }
    }
}

#[tokio::main]
async fn main() {
    let pool = get_pool().await;

    pool.execute("CREATE TABLE IF NOT EXISTS fruits(name VARCHAR(256) NOT NULL PRIMARY KEY, price BIGINT UNSIGNED NOT NULL)").await.unwrap();


    let app = Router::new()
        .route("/fruit/:fruit_name", get(fruit))
        .route("/create_fruit/:fruit_name/:price", get(create_fruit))
        .route("/fruits", get(list_fruits));

    let addr = SocketAddr::from(([127, 0, 0, 69], 3000));

    let listener = TcpListener::bind(addr)
        .await
        .expect("Failed to start tcp server!");

    axum::serve(listener, app).await.unwrap();
    

}
```
