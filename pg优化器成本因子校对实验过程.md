2022.6.15

在服务器上实验一篇有关pg优化器成本因子校对的实验，数据集为imdb，对象为其中的四个表格。

movie_companies，2609129行；

movie_info，14835720行；

movie_keyword,4523930行；

title,2528312行；


#1.建表：


imdb3=# CREATE TABLE movie_companies (
imdb3(#     id integer NOT NULL PRIMARY KEY,
imdb3(#     movie_id integer NOT NULL,
imdb3(#     company_id integer NOT NULL,
imdb3(#     company_type_id integer NOT NULL,
imdb3(#     note text
imdb3(# );
CREATE TABLE
imdb3=# CREATE TABLE movie_info (
imdb3(#     id integer NOT NULL PRIMARY KEY,
imdb3(#     movie_id integer NOT NULL,
imdb3(#     info_type_id integer NOT NULL,
imdb3(#     info text NOT NULL,
imdb3(#     note text
imdb3(# );
CREATE TABLE
imdb3=# CREATE TABLE movie_keyword (
imdb3(#     id integer NOT NULL PRIMARY KEY,
imdb3(#     movie_id integer NOT NULL,
imdb3(#     keyword_id integer NOT NULL
imdb3(# );
CREATE TABLE
imdb3=# CREATE TABLE title (
imdb3(#     id integer NOT NULL PRIMARY KEY,
imdb3(#     title text NOT NULL,
imdb3(#     imdb_index character varying(12),
imdb3(#     kind_id integer NOT NULL,
imdb3(#     production_year integer,
imdb3(#     imdb_id integer,
imdb3(#     phonetic_code character varying(5),
imdb3(#     episode_of_id integer,
imdb3(#     season_nr integer,
imdb3(#     episode_nr integer,
imdb3(#     series_years character varying(49),
imdb3(#     md5sum character varying(32)
imdb3(# );
