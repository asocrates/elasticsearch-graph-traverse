# elasticsearch-graph-traverse

At Voxer, the profile information (first name, last name, username/handle, phone, email,...) of each user is indexed in an Elasticsearch instance.

In the absence of information of a given user's social graph, if a user enters a type-down search for their friend "Joe Pope" with the string "Joe" then elasticsearch returns users with first name, last name and usernames that contain the string "Joe". Sorting is done by score, then alphabetically.

Notice the boosting for first and last names -- first name matches are treated more favorably than the last.

The query is a "bool" query. The advantage of this is that, in a single query, the query can be checked if it satifies various conditions. The satisfaction of each condition is then boosted by a given amount, the value of each boost can then be tuned empirically.

Graph Search is implemented by the "bool":"should":"terms":"_id" portion of the es query. The array of userids are the friend of friends of the user in question.

The friends of friends list is computed "offline" i.e., during signup, start sessions, weekly, etc.

This query structure is highly efficient and at Voxer, returns sorted search results in 100 ms or so.

