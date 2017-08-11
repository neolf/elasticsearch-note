##设置自己的
下面的Analyzer所列参数不能直接配置，必须生成自己Analyzer后使用
```
PUT /my_test
{
    "settings": {
        "analysis": {
            "filter": {
                "my_stop": {
                    "type":       "stop",
                    "stopwords": ["and", "is", "交"]
                }
            }
        }
    }
}
```



## Standard Analyzer

> standard

Standard Analyzer由Standard Tokenizer和Standard Token Filter, Lower Case Token Filter, and Stop Token Filter组成

Standard Tokenizer 提供基于语法的标记器，它对大多数欧洲语言文档处理良好（中文基本上只能分割成单字）。

```
POST /my_test/_analyze?analyzer=standard&pretty=true
{
"text":"An analyzer of type standard is built using the Standard Tokenizer with the Standard Token Filter, Lower Case Token Filter, and Stop Token Filter机器学习 作为 一个交叉广度大，各学科融合深的学科"
}
```


## Simple Analyzer
> simple

simple analyzer由Lower Case Tokenizer构成。

只提供简单的大小写转换，空格、标点分割，中文不分割。

```
POST /my_test/_analyze?analyzer=simple&pretty=true
{
"text":"An analyzer of type standard is built using the Standard Tokenizer with the Standard Token Filter, Lower Case Token Filter, and Stop Token Filter机器学习 作为 一个交叉广度大，各学科融合深的学科"
}
```



## Whitespace Analyzer
> whitespace

whitespace analyzer由Whitespace Tokenizer构成.

只提供空格分割，保留大小写，中文不分割


## Stop Analyzer

stopwords 将不会出现

```
POST /my_test
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_stop_analyzer1": {
          "type":      "stop",
          "stopwords": ["and", "is", "交"]
        }
      }
    }
  }
}

POST /my_test/_analyze?pretty=true
{
    "analyzer":"my_stop_analyzer1",
"text":"An analyzer of type standard is built using the Standard Tokenizer with the Standard Token Filter, Lower Case Token Filter, and Stop Token Filter机器学习 作为 一个 交 叉 广 度 大，各学科融合深的学科"
}
```


## Keyword Analyzer

keyword analyzer 会认为当前的字符串是一个key，因此不会做任何操作

## Pattern Analyzer

pattern analyzer 会创建一个正则表达式用来获取需要的信息

## Language Analyzers
语言分析器，支持如下语言: arabic, armenian, basque, brazilian, bulgarian, catalan, cjk, czech, danish, dutch, english, finnish, french, galician, german, greek, hindi, hungarian, indonesian, irish, italian, latvian, lithuanian, norwegian, persian, portuguese, romanian, russian, sorani, spanish, swedish, turkish, thai.


## Snowball Analyzer
提供各种转换包括大小写、单复数、词性等转换，注意如果字段经过这种转换才保存的话，搜索时也需要转换

## Custom Analyzer
Custom analyzer 可以让你自定义所有细节
```
index :
    analysis :
        analyzer :
            myAnalyzer2 :
                type : custom
                tokenizer : myTokenizer1
                filter : [myTokenFilter1, myTokenFilter2]
                char_filter : [my_html]
                position_increment_gap: 256
        tokenizer :
            myTokenizer1 :
                type : standard
                max_token_length : 900
        filter :
            myTokenFilter1 :
                type : stop
                stopwords : [stop1, stop2, stop3, stop4]
            myTokenFilter2 :
                type : length
                min : 0
                max : 2000
        char_filter :
              my_html :
                type : html_strip
                escaped_tags : [xxx, yyy]
                read_ahead : 1024
```