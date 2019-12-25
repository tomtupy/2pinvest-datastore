# 2pinvest-datastore
Stands up the following data-stores
* [Hashicorp Vault](https://www.vaultproject.io/)
* [InfluxDB](https://www.influxdata.com/products/influxdb-overview/)
* [Postgres 12](https://www.postgresql.org/)

## Run
```docker-compose build```
```docker-compose up -d```

## Postgres Setup
### IPO lockup period expirations table
```
-- Table: public.lockup_period_expirations

-- DROP TABLE public.lockup_period_expirations;

CREATE TABLE public.lockup_period_expirations
(
    id serial NOT NULL,
    expiration_date date NOT NULL,
    date_priced date,
    price_usd double precision,
    shares integer,
    offer_amount_usd double precision,
    symbol character varying(10) COLLATE pg_catalog."default" NOT NULL,
    company_name text COLLATE pg_catalog."default" NOT NULL,
    market text COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT lockup_period_expirations_pkey PRIMARY KEY (id)
)

TABLESPACE pg_default;

ALTER TABLE public.lockup_period_expirations
    OWNER to postgres;

-- Index: symbol_expiration

-- DROP INDEX public.symbol_expiration;

CREATE UNIQUE INDEX symbol_expiration
    ON public.lockup_period_expirations USING btree
    (symbol COLLATE pg_catalog."default", expiration_date)
    TABLESPACE pg_default;
```
