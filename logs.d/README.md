# ELK7

```sh
diff shakes-mapping.json shakes-mapping_7.0.json
3,9c3,7
<  		"doc" : {
< 			"properties" : {
< 				"speaker" : {"type": "keyword" },
< 				"play_name" : {"type": "keyword" },
< 				"line_id" : { "type" : "integer" },
< 				"speech_number" : { "type" : "integer" }
< 			}
---
> 		"properties" : {
> 			"speaker" : {"type": "keyword" },
> 			"play_name" : {"type": "keyword" },
> 			"line_id" : { "type" : "integer" },
> 			"speech_number" : { "type" : "integer" }
```

## Mapping
curl -H "Content-Type: application/json" -XPUT http:/tubasan.local:9200/shakespeare --data-binary @shakes-mapping_7.0.json

```sh
$ curl -XGET 'tubasan.local:9200/shakespeare/_mapping?pretty'
{
  "shakespeare" : {
    "mappings" : {
      "properties" : {
        "line_id" : {
          "type" : "integer"
        },
        "play_name" : {
          "type" : "keyword"
        },
        "speaker" : {
          "type" : "keyword"
        },
        "speech_number" : {
          "type" : "integer"
        }
      }
    }
  }
}
```


```sh
$ wget http://media.sundog-soft.com/es7/shakespeare_7.0.json
$ wc -l shakespeare_7.0.json
  222792 shakespeare_7.0.json

$ curl -H 'Content-Type: application/json' -XPOST 'tubasan.local:9200/shakespere/doc/_bulk?pretty' --data-binary @shakespeare_7.0.json


```

### MappingError
```
    {
      "index" : {
        "_index" : "shakespeare",
        "_type" : "doc",
        "_id" : "111395",
        "status" : 400,
        "error" : {
          "type" : "illegal_argument_exception",
          "reason" : "mapper [play_name] cannot be changed from type [keyword] to [text]"
        }
      }
```

```sh
$ curl -H 'Content-Type: application/json' -XGET 'tubasan.local:9200/shakespeare/_search?pretty' -d '
{
  "query": {
    "match_phrase": {
      "text_entry": "to be or not to be"
    }
  }
}'
```

