# Dataset Olist (Brazilian E-Commerce Public Dataset)

Cópia, para uso em aula, do **Brazilian E-Commerce Public Dataset by Olist**, publicado pela Olist no Kaggle:
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

São cerca de 100 mil pedidos reais (anonimizados) feitos entre 2016 e 2018 em vários marketplaces brasileiros. Este repositório existe para que os laboratórios baixem os arquivos com `curl`, sem precisar de conta no Kaggle.

## Licença e atribuição

Os dados são da **Olist** e estão sob a licença [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/). O texto completo está em [LICENSE](LICENSE).

- **Atribuição:** Olist, *Brazilian E-Commerce Public Dataset by Olist*, Kaggle.
- **Uso não comercial:** apenas fins educacionais e de pesquisa.
- **Compartilha igual:** qualquer redistribuição destes dados deve manter a mesma licença.

Este repositório não tem vínculo com a Olist.

**Única modificação em relação ao original:** cada CSV foi comprimido com `gzip -9`. O conteúdo dos arquivos é idêntico ao publicado no Kaggle (versão de 2021-10-01).

## Arquivos

Todos em `data/`, CSV separado por vírgula, com cabeçalho, UTF-8, comprimido em gzip.

| Arquivo | Linhas | Colunas |
|---|---:|---|
| `olist_orders_dataset.csv.gz` | 99.441 | order_id, customer_id, order_status, order_purchase_timestamp, order_approved_at, order_delivered_carrier_date, order_delivered_customer_date, order_estimated_delivery_date |
| `olist_order_items_dataset.csv.gz` | 112.650 | order_id, order_item_id, product_id, seller_id, shipping_limit_date, price, freight_value |
| `olist_order_payments_dataset.csv.gz` | 103.886 | order_id, payment_sequential, payment_type, payment_installments, payment_value |
| `olist_order_reviews_dataset.csv.gz` | 99.224 | review_id, order_id, review_score, review_comment_title, review_comment_message, review_creation_date, review_answer_timestamp |
| `olist_customers_dataset.csv.gz` | 99.441 | customer_id, customer_unique_id, customer_zip_code_prefix, customer_city, customer_state |
| `olist_sellers_dataset.csv.gz` | 3.095 | seller_id, seller_zip_code_prefix, seller_city, seller_state |
| `olist_products_dataset.csv.gz` | 32.951 | product_id, product_category_name, product_name_lenght, product_description_lenght, product_photos_qty, product_weight_g, product_length_cm, product_height_cm, product_width_cm |
| `olist_geolocation_dataset.csv.gz` | 1.000.163 | geolocation_zip_code_prefix, geolocation_lat, geolocation_lng, geolocation_city, geolocation_state |
| `product_category_name_translation.csv.gz` | 71 | product_category_name, product_category_name_english |

Os nomes de colunas são os originais (inclusive os erros de digitação `lenght`).

## Relacionamentos

- `orders.customer_id` → `customers.customer_id`. Cada pedido tem um `customer_id` próprio; o cliente real é o `customer_unique_id`.
- `order_items`, `order_payments` e `order_reviews` se ligam a `orders` por `order_id`.
- `order_items.product_id` → `products`; `order_items.seller_id` → `sellers`.
- O prefixo de CEP liga `customers` e `sellers` a `geolocation`.

## Exemplo

```sh
curl -sLO https://raw.githubusercontent.com/infobarbosa/dataset-olist/main/data/olist_orders_dataset.csv.gz
duckdb -c "SELECT order_status, count(*) FROM 'olist_orders_dataset.csv.gz' GROUP BY ALL ORDER BY 2 DESC"
```
