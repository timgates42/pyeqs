# PyEQS [![Build Status](https://travis-ci.org/Yipit/pyeqs.svg)](https://travis-ci.org/Yipit/pyeqs) [![Coverage Status](https://coveralls.io/repos/Yipit/pyeqs/badge.png)](https://coveralls.io/r/Yipit/pyeqs)

#### Python Elasticsearch QuerySets

A python library to simplify building complex Elasticsearch JSON queries.  Based on the Django QuerySet API, backed by the [official python elasticsearch library](https://github.com/elasticsearch/elasticsearch-py).  Supports Elasticsearch `1.0+`.

## Installation

```bash
pip install pyeqs
```

## Usage

#### Simple querying

```python
from pyeqs import QuerySet
qs = QuerySet("127.0.0.1", index="my_index")
print qs._query
"""
{
  'query': {
    'match_all': {}
  }
}"""
```

```python
from pyeqs import QuerySet
qs = QuerySet("127.0.0.1", query="cheese", index="my_index")
print qs._query
"""
{
  'query': {
    'query_string': {
      'query': 'cheese'
    }
  }
}"""
```

#### Simple Filtering

```python
from pyeqs import QuerySet
from pyeqs.dsl import Term
qs = QuerySet("127.0.0.1", index="my_index")
qs.filter(Term("foo", "bar"))
print qs._query
"""
{
  'query': {
    'filtered': {
      'filter': {
        'and': [
          {
            'term': {
              'foo': 'bar'
            }
          }
        ]
      },
      'query': {
        'match_all': {}
      }
    }
  }
}"""
```

#### Complex Filtering

```python
from pyeqs import QuerySet
from pyeqs.dsl import Term, Type
qs = QuerySet("127.0.0.1", index="my_index")
qs.filter(Term("foo", "bar")).filter(Type("baz"))
print qs._query
"""
{
  'query': {
    'filtered': {
      'filter': {
        'and': [
          {
            'term': {
              'foo': 'bar'
            }
          },
          {
            'type': {
              'value': 'baz'
            }
          }
        ]
      },
      'query': {
        'match_all': {}
      }
    }
  }
}"""
```
