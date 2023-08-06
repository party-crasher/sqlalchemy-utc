# SQLAlchemy-Utc

[![Pypi](https://badge.fury.io/py/SQLAlchemy-Utc.svg?)](https://pypi.python.org/pypi/SQLAlchemy-Utc)
[![Travis CI](https://travis-ci.com/spoqa/sqlalchemy-utc.svg?branch=master)](https://travis-ci.com/spoqa/sqlalchemy-utc)
[![Code Coverage](https://codecov.io/github/spoqa/sqlalchemy-utc/coverage.svg?branch=master)](https://codecov.io/github/spoqa/sqlalchemy-utc?branch=master)

This package provides a drop-in replacement of SQLAlchemy's built-in [`DateTime`](http://docs.sqlalchemy.org/en/latest/core/type_basics.html#sqlalchemy.types.DateTime)
type with `timezone=True` option enabled.  Although SQLAlchemy's built-in
`DateTime` type provides `timezone=True` option, since some vendors like
SQLite and MySQL don't provide `timestamptz` data type, the option doesn't
make any effect on these vendors.

`UtcDateTime` type is equivalent to the built-in `DateTime` with
`timezone=True` option enabled on vendors that support `timestamptz`
e.g. PostgreSQL, but on SQLite or MySQL, it shifts all `datetime.datetime`
values to UTC offset before store them, and returns always aware
`datetime.datetime` values through result sets.

Long story short, `UtcDateTime` does:

- take only aware `datetime.datetime`,
- return only aware `datetime.datetime`,
- never take or return naive `datetime.datetime`,
- ensure timestamps in database always to be encoded in UTC, and
- work as you'd expect.

A SQLAlchemy helper function, `utcnow()`, is provided as an alternative
to `func.now()` for generating `UtcDateTime` values on the server. For
example: `Column('time', UtcDateTime(), default=utcnow())`.

Written by [Hong Minhee](https://hongminhee.org/) at [Spoqa](http://www.spoqa.com/), and distributed under MIT license.