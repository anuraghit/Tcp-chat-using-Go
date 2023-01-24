### Question
Write a TCP/IPv4 server that provides the below functionality.


### Part 1

1. Be able to communicate over TCP with a client such as `nc` (netcat),
`telnet`, or a custom TCP client.

2. Provide echo functionality:
Command: "ECHO <string>"
Expected response: "!200 <string>"


### Part 2

3. Allow data to be stored:
Command: "SET <key> <value>"
- if successful: "!200"
- if store is full: "!500"
- if syntax error: "!400"

4. Allow data to be retrieved:
Command: "GET <key>"
Expected response: "!200 <value>"
Errors:
- if key not found (ie., not previously set): "!404"
- if syntax error: "!400"

5. Empty the contents of the datastore:
Command: "FLUSH"
Expected response: "!200" if successful.


### Part 3

6. Allow setting tags with keys:
Command: "SET <key> <value> <tag1> <tag2> <...tagN>"
Expected behaviour is same as (3)
7. Allow data to be retrieved with tags:
Command: "GETWITHTAGS <key>"
Expected response: "!200 <value> [<tag1> <tag2> <...tagN>]"
Other behaviour similar to (4)

8. List keys with a specific tag:
Command: "LISTKEYSWITHTAG EXACT <tagname>"
Expected response: "!200 [<key1> <key2> <...key3>]"
Errors:
- if tag not found (or there exists no keys with that tag): "!404"


### Part 4

9a. Save the datastore to disk on the server referenced by <name>:
Command: "SAVE <name>"
Expected response: 
- "!200" if successfully saved. If a previous save by the same name exists,
  this operation will silently overwrite it.
- "!500" if an error occurs.

9b. Load your datastore from a previously saved copy:
Command: "LOAD <name>"
Expected response:
- "!200" if load succeeds. Any existing data is first deleted before data
  is loaded into your data store.
- "!404" if <name> doesn't exist.
- "!500" if an error occurs.


### Part 5

10. Aggregation functionality:
Support COUNT, MAX, MIN, AVG, SUM.
Command: "COUNT <tag>"
Response: "!200 <result-value>", where result-value is the result of the
aggregation function.
For example, if the current state of the datastore was:
---
key1 30 tagR
key2 40 tagQ
key3 20 tagR
---
then "AVG tagR" would respond with "!200 25".


### Notes
1. It isn't necessary to retain the values in the datastore
   between restarts.
2. You will be able to test your code by running the
   `checker2.py` script. See the top of the script for
   instructions on how to run it. 
3. Ensure that your program follows the spec exactly since the
   checker is automated.
4. Test often.
5. Listen for instructions on the Zoom call.


### Table of control codes
| meaning                                  | control code|
| --------                                 |   -------- |
| generic error                            |        500 |
| invalid input or parsing error           |        400 |
| input too large                          |        413 |
| key not found                            |        404 |
| error loading .db - no such file?        |        404 |
| OK (but no data response)                |        200 |

In general,
2xx for OK status code
4xx for client errors
5xx for server errors


### Wire format
1. Requests and responses are terminated by a newline character ("\n").
2. General request format:
        "COMMAND arg1 arg2 ..."
  - arg's are optional and dependent on the COMMAND.
3. General response format: 
        "!CONTROLCODE retval1 retval2 ..."
  - retval's are optional and depends on the COMMAND
    and whether there was an error.
4. Since tokens are separated by a space character, spaces are not allowed
   in keys, values, or tags.
(END)
