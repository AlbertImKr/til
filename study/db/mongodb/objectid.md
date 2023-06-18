# ObjectId

The 12-byte Objectid consists of:

* A 4-byte timestamp, representing the ObjectId's creation, measured in seconds since the Unix epoch.
* A 5-byte random value generated once per process. This random value is unique to the machine and process
* A 3-byte incremeting counter, initialized to a random value.

For timestamp and counter values, the most significant bytes appear first in byte sequence(big-endian). This is unlike other BSON values, where the least significant bytes appear first (little-endian).

* BSON \[bee · sahn], short for Bin­ary JSON, is a bin­ary-en­coded seri­al­iz­a­tion of JSON-like doc­u­ments. Like JSON, BSON sup­ports the em­bed­ding of doc­u­ments and ar­rays with­in oth­er doc­u­ments and ar­rays. BSON also con­tains ex­ten­sions that al­low rep­res­ent­a­tion of data types that are not part of the JSON spec. For ex­ample, BSON has a Date type and a BinData type.
