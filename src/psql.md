PostgreSQL cheat sheet
===

Working with `psql`
---

Get commands help

    \?

<b>L</b>ist databases

    \l[ist]

Created database

    CREATE DATABASE <database-name>;

<b>C</b>onnect to specific database

    \c[onnect] <database-name>;

List users (<b>d</b>escribe <b>u</b>sers)

    \du

Show all schemas (<b>d</b>escribe <b>n</b>ames)

    \dn

Show table information (<b>d</b>escribe table)

    \d+  <table-name>

Drop table

    DROP TABLE <table-name>;

Create table example
---

    CREATE TABLE employees (
        id SERIAL PRIMARY KEY NOT NULL,
        name VARCHAR(64) NOT NULL DEFAULT 'John Doe',
        salary MONEY NOT NULL DEFAULT 10000.00,
        started_at DATE NOT NULL DEFAULT '2018-01-01'
    );

    \x
    Expanded display is on.

    select * from employees;
    -[ RECORD 1 ]----------
    id         | 1
    name       | Kirill
    salary     | $5,000.00
    started_at | 2000-01-01

Data types
---

[Reference](https://www.postgresql.org/docs/current/datatype.html) (official, up to date)

The following cheat sheet was extracted from [here](https://tableplus.io/blog/2018/06/postgresql-data-types.html)

### Numeric types

| Name     | Storage Size | Range                                                                                    |
| ---      | ---          | ---                                                                                      |
| smallint | 2 bytes      | -32768 to +32767                                                                         |
| integer  | 4 bytes      | -2147483648 to +2147483647                                                               |
| bigint   | 4 bytes      | -9223372036854775808 to 9223372036854775807                                              |
| decimal  | variable     | up to 131072 digits before the decimal point; up to 16383 digits after the decimal point |
| numeric  | variable     | up to 131072 digits before the decimal point; up to 16383 digits after the decimal point |
| real     | 4 bytes      | 6 decimal digits precision                                                               |
| double   | 8 bytes      | 15 decimal digits precision                                                              |

### Serial types

| Name        | Storage Size | Range                    |
| ---         | ---          | ---                      |
| smallserial | 2 bytes      | 1 to 32767               |
| serial      | 4 bytes      | 1 to 2147483647          |
| bigserial   | 8 bytes      | 1 to 9223372036854775807 |

### Monetary types

| Name  | Storage Size | Range                                         |
| ---   | ---          | ---                                           |
| money | 8 bytes      | -92233720368547758.08 to 92233720368547758.07 |

### Strings

| Name       | Description                |
| ---        | ---                        |
| varchar(n) | variable-length with limit |
| char(n)    | fixed-length, blank padded |
| text       | variable unlimited length  |

### Binary

| Name  | Storage Size                            | Description                   |
| ---   | ---                                     | ---                           |
| bytea | 1 to 4 bytes plus size of binary string | variable-length binary string |


### Datetime

| Name                       | Size     | Range                               | Resolution                |
| ---                        | ---      | ---                                 | ---                       |
| timestamp without timezone | 8 bytes  | 4713 BC to 294276 AD                | 1 microsecond / 14 digits |
| timestamp with timezone    | 8 bytes  | 4713 BC to 294276 AD                | 1 microsecond / 14 digits |
| date                       | 4 bytes  | 4713 BC to 5874897 AD               | 1 day                     |
| time without timezone      | 8 bytes  | 00:00:00 to 24:00:00                | 1 microsecond / 14 digits |
| time with timezone         | 12 bytes | 00:00:00+1459 to 24:00:00-1459      | 1 microsecond / 14 digits |
| interval                   | 12 bytes | -178000000 years to 178000000 years | 1 microsecond / 14 digits |

### Boolean

| Name    | Size   |
| ---     | ---    |
| boolean | 1 byte |

### Goemetry

| Name    | Size           | Representation                                    | Format                            |
| ---     | ---            | ---                                               | ---                               |
| point   | 16 bytes       | point on a plane                                  | (x,y)                             |
| lseg    | 32 bytes       | finite line segment                               | ((x1, y1), (x2, y2))              |
| box     | 32 bytes       | rectangular box                                   | ((x1, y1), (x2, y2))              |
| path    | 16 + 16n bytes | closed path (similar to polygon, n = total points | ((x1, y1), (x2, y2), …, (xn, yn)) |
| path    | 16 + 16n bytes | open path, n = total points                       | [(x1, y1), (x2, y2), …, (xn, yn)] |
| polygon | 40 bytes + 16n | polygon                                           | ((x1, y1), (x2, y2), …, (xn, yn)) |
| circle  | 24 bytes       | circle – center point and radius                  | <(x, y), r>                       |

### Networking

| Name    | Storage Size  | Description                    |
| ---     | ---           | ---                            |
| cidr    | 7 or 19 bytes | IPv4 or IPv6 networks          |
| inet    | 7 or 19 bytes | IPv4 or IPv6 hosts or networks |
| macaddr | 6 bytes       | MAC addresses                  |

### Bit strings

| Name           | Storage Size          | Description                           |
| ---            | ---                   | ---                                   |
| bit(n)         | y + ceil(n / 8) bytes | stores exactly n 0s and 1s y = 5 or 8 |
| bit varying(n) | y + ceil(n / 8) bytes | stores up to n 0s and 1s y = 5 or 8   |
| bit varying    | variable              | stores unlimited number of 0s and 1s  |

