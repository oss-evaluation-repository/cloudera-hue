---
title: "Querying"
date: 2019-03-13T18:28:09-07:00
draft: false
weight: 2
---

Hue's goal is to make Databases & Datawarehouses querying easy and productive.

Several apps, each one specialized in a certain type of querying are available. Data sources can be explored first via the [browsers](/user/browsing/).

* The Editor shines for SQL queries but also supports job submissions. It comes with an intelligent autocomplete, risk alerts and self service troubleshooting.
* The Editor is also available in Notebook mode for quickly executing light programming snippets.
* Dashboards are focusing on visualizing indexed data but can also query SQL databases.

The configuration of the connectors is currently done by the [Administrator](/administrator/configuration/connectors/).

## Editor

![Editor](https://cdn.gethue.com/uploads/2019/08/hue_4.5.png)

### Running Queries

SQL query execution is the primary use case of the Editor.

1.  The currently selected statement has a **left blue** border. To execute a portion of a query, highlight one or more query
    statements.
2.  Click **Execute**. The Query Results window appears
    -   There is a Log caret on the left of the progress bar
    -   Expand the **Columns** by clicking on the column label will scroll to the column. Names and types can be filtered
    -   Select the **Chart** icon to plot the results
    -   To expand a row, click on the row number
    -   To lock a row, click on the lock icon in the row number column
    -   Search either by clicking on the magnifier icon on the results tab, or pressing `Ctrl/Cmd + F`
    -   [See more how to refine your results](http://gethue.com/new-features-in-the-sql-results-grid-in-hive-and-impala/).

3.  If there are **multiple statements** in the query (separated by semi-colons), click Next in the
    multi-statement query pane to execute the remaining statements.

When you have multiple statements it's enough to put the cursor in the statement you want to execute, the active statement is indicated with a blue gutter marking.

**Note**: Use `CTRL/Cmd + ENTER` to execute queries.

**Note**: On top of the logs panel, there is a link to open the query profile in the [Query Browser](/user/browsing/#impala-queries).

### Running Jobs

In addition to SQL, these types of jobs are supported:

* [Apache Pig](https://pig.apache.org/) Latin instructions to load/merge data to perform ETL or Analytics.
* Running an SQL import from a traditional relational database via an [Apache Sqoop](https://sqoop.apache.org/) command.
* Regular Java, MapReduce, [shell script](http://gethue.com/use-the-shell-action-in-oozie/).
* [Spark](http://gethue.com/use-the-spark-action-in-oozie/) Jar or Python script to trial and error them in YARN via [Oozie](http://gethue.com/how-to-schedule-spark-jobs-with-spark-on-yarn-and-oozie/) or Livy.


### Downloading Results

There are several ways you can export results of a query.

The most common:

* Download to your computer as a CSV or XLS
* Copy the currently fetched rows to the clipboard

Two of them offer greater scalability:

 * Export to an empty folder on your cluster's file system.
 * Export to a table. You can choose an already existing table or a new one.

![Downloading and Exporting Results](https://cdn.gethue.com/uploads/2019/04/editor_export_results.png)

### Autocomplete

To make your SQL editing experience, Hue comes with one of the best SQL autocomplete on the planet. The new autocompleter knows all the ins and outs of the Hive and Impala SQL dialects and will suggest keywords, functions, columns, tables, databases, etc. based on the structure of the statement and the position of the cursor.

The result is improved completion throughout. We now have completion for more than just SELECT statements, it will help you with the other DDL and DML statements too, INSERT, CREATE, ALTER, DROP etc.

![Autocomplete and context assist](https://cdn.gethue.com/uploads/2017/07/hue_4_assistant_2.gif)

**Smart column suggestions**

If multiple tables appear in the FROM clause, including derived and joined tables, it will merge the columns from all the tables and add the proper prefixes where needed. It also knows about your aliases, lateral views and complex types and will include those. It will now automatically backtick any reserved words or exotic column names where needed to prevent any mistakes.

**Smart keyword completion**

The autocompleter suggests keywords based on where the cursor is positioned in the statement. Where possible it will even suggest more than one word at at time, like in the case of IF NOT EXISTS, no one likes to type too much right? In the parts where order matters but the keywords are optional, for instance after FROM tbl, it will list the keyword suggestions in the order they are expected with the first expected one on top. So after FROM tbl the WHERE keyword is listed above GROUP BY etc.

**Functions**

The improved autocompleter will now suggest functions, for each function suggestion an additional panel is added in the autocomplete dropdown showing the documentation and the signature of the function. The autocompleter know about the expected types for the arguments and will only suggest the columns or functions that match the argument at the cursor position in the argument list.

<img src="https://cdn.gethue.com/uploads/2017/07/hue_4_functions.png" alt="SQL functions reference" height="400"/>

**Sub-queries, correlated or not**

When editing subqueries it will only make suggestions within the scope of the subquery. For correlated subqueries the outside tables are also taken into account.

**Context popup**

Right click on any fragement of the queries (e.g. a table name) to gets all its metadata information. This is a handy shortcut to get more description or check what types of values are contained in the table or columns.

It’s quite handy to be able to look at column samples while writing a query to see what type of values you can expect. Hue now has the ability to perform some operations on the sample data, you can now view distinct values as well as min and max values. Expect to see more operations in coming releases.

![Sample column popup](https://cdn.gethue.com/uploads/2018/10/sample_context_operations.gif)

**Syntax checker**

A little red underline will display the incorrect syntax so that the query can be fixed before submitting. A right click offers suggestions.

![Syntax checker](https://cdn.gethue.com/uploads/2018/01/syntax_checkerhigh.png)

![Syntax checker](https://cdn.gethue.com/uploads/2018/01/checker_help.png)

**Advanced Settings**

The live autocompletion is fine-tuned for a better experience advanced settings an be accessed via `CTRL +` , (or on Mac `CMD + ,`) or clicking on the '?' icon.

The autocompleter talks to the backend to get data for tables and databases etc and caches it to keep it quick. Clicking on the refresh icon in the left assist will clear the cache. This can be useful if a new table was created outside of Hue and is not yet showing-up (Hue will regularly clear his cache to automatically pick-up metadata changes done outside of Hue).

### Sharing

Any query can be shared with permissions, as detailed in the [concepts](/user/concepts).

### Language reference

You can find the Language Reference in the right assist panel. The right panel itself has a new look with icons on the left hand side and can be minimised by clicking the active icon.

The filter input on top will only filter on the topic titles in this initial version. Below is an example on how to find documentation about joins in select statements.

![Language Reference Panel](https://cdn.gethue.com/uploads/2018/10/impala_lang_ref_joins.gif)

While editing a statement there’s a quicker way to find the language reference for the current statement type, just right-click the first word and the reference appears in a popover below:

![Language Reference context](https://cdn.gethue.com/uploads/2018/10/impala_lang_ref_context.png)

### Variables
Variables are used to easily configure parameters in a query. They are ideal for saving reports that can be shared or executed repetitively:

**Single Valued**

    select * from web_logs where country_code = "${country_code}"

![Single valued variable](https://cdn.gethue.com/uploads/2017/10/var_defaults.png)

**The variable can have a default value**

    select * from web_logs where country_code = "${country_code=US}"

**Multi Valued**

    select * from web_logs where country_code = "${country_code=CA, FR, US}"

**In addition, the displayed text for multi valued variables can be changed**

    select * from web_logs where country_code = "${country_code=CA(Canada), FR(France), US(United States)}"

![Multi valued variables](https://cdn.gethue.com/uploads/2018/04/variables_multi.png)

**For values that are not textual, omit the quotes.**

    select * from boolean_table where boolean_column = ${boolean_column}

### Charting

These visualizations are convenient for plotting chronological data or when subsets of rows have the same attribute: they will be stacked together.

* Pie
* Bar/Line with pivot
* Timeline
* Scattered plot
* Maps (Marker and Gradient)

![Charts](https://cdn.gethue.com/uploads/2019/04/editor_charting.png)

### Query troubleshooting

#### Pre-query execution

**Popular values**

The autocompleter will suggest popular tables, columns, filters, joins, group by, order by etc. based on metadata from Navigator Optimizer. A new “Popular” tab has been added to the autocomplete result dropdown which will be shown when there are popular suggestions available.

This is particularly useful for doing joins on unknown datasets or getting the most interesting columns of tables with hundreds of them.

![Popular joins suggestion](https://cdn.gethue.com/uploads/2017/07/hue_4_query_joins.png)
![Popular columns suggestion](https://cdn.gethue.com/uploads/2017/07/hue_4_popular_filter_agg.png)


**Risk alerts**

While editing, Hue will run your queries through Navigator Optimizer in the background to identify potential risks that could affect the performance of your query. If a risk is identified an exclamation mark is shown above the query editor and suggestions on how to improve it is displayed in the lower part of the right assistant panel.

![Query Risk alerts](https://cdn.gethue.com/uploads/2017/07/hue_4_risk_6.gif)

#### During execution

The Query Browser details the plan of the query and the bottle necks. When detected, "Health" risks are listed with suggestions on how to fix them.

![Pretty Query Profile](https://cdn.gethue.com/uploads/2019/03/Screen-Shot-2019-03-07-at-11.40.24-AM.png)

#### Post-query execution

A new experimental panel when enabled can offer post risk analysis and recommendation on how to tweak the query for better speed.

### Presentation Mode

Turns a list of semi-colon separated queries into an interactive presentation by clicking on the 'Dashboard' icon. It is great for doing demos or reporting.

### Dark mode

Initially this mode is limited to the actual editor area and we’re considering extending this to cover all of Hue.

![Editor Dark Mode](https://cdn.gethue.com/uploads/2018/10/editor_dark_mode.png)

To toggle the dark mode you can either press `Ctrl-Alt-T` or `Command-Option-T` on Mac while the editor has focus. Alternatively you can control this through the settings menu which is shown by pressing `Ctrl-`, or `Command-`, on Mac.

### Scheduling

Scheduling of queries is currently done via Apache Oozie and will be open to other schedulers with [HUE-3797](https://issues.cloudera.org/browse/HUE-3797).

![Oozie workflows](https://cdn.gethue.comuploads/2016/04/hue-workflows.png)


## Dashboards

Dashboards provide an interactive way to query indexed data quickly and easily. No programming is required and the analysis is done by drag & drops and clicks.

![Search Full](https://cdn.gethue.com/uploads/2015/08/search-full-mode.png)

Widgets are interconnected together. This is great for exploring new datasets or monitoring without having to type.

![Analytics dimensions](https://cdn.gethue.com/uploads/2018/08/dashboard_layout_dnd.gif)

The best supported engine is Apache Solr, then support for SQL databases like Apache Hive and Apache Impala is getting better. To help add more databases, feel free to check the [dashboard connector](/developer/connectors/#dashboard) section.

These tutorials showcase the capabilities:

* The top search bar offers a [full autocomplete](http://gethue.com/intuitively-discovering-and-exploring-a-wine-dataset-with-the-dynamic-dashboards/) on all the values of the index
* Seeing [real time data](http://gethue.com/build-a-real-time-analytic-dashboard-with-solr-search-and-spark-streaming/)
* Comprehensive demo of [BikeShare data visualization post](http://gethue.com/bay-area-bikeshare-data-analysis-with-search-and-spark-notebook/)


### Analytics facets

Drill down the dimensions of the datasets and apply aggregates functions on top of it:

![Analytics dimensions](https://cdn.gethue.com/uploads/2018/08/dashboard_layout_dimensions.gif)

Some facets can be nested:

![Nested Analytics facets](https://cdn.gethue.com/uploads/2015/08/search-nested-facet-1024x304.png)
![Nested Analytics Counts](https://cdn.gethue.com/uploads/2015/08/search-hit-widget.png)


### Autocomplete
The top bar support faceted and free word text search, with autocompletion.

![Search Autocomplete](https://cdn.gethue.com/uploads/2018/01/dashboard_autocomplete.png)

### Marker Map
Points close to each other are grouped together and will expand when zooming-in. A Yelp-like search filtering experience can also be created by checking the box.

![Marker Map](https://cdn.gethue.com/uploads/2015/08/search-marker-map.png)

### Edit records

Indexed records can be directly edited in the Grid or HTML widgets by admins.

### Links

Links to the original documents can also be inserted. Add to the record a field named ‘link-meta’ that contains some json describing the URL or address of a table or file that can be open in the HBase Browser, Metastore App or File Browser:

Any link

    {'type': 'link', 'link': 'gethue.com'}

HBase Browser

    {'type': 'hbase', 'table': 'document_demo', 'row_key': '20150527'}
    {'type': 'hbase', 'table': 'document_demo', 'row_key': '20150527', 'fam': 'f1'}
    {'type': 'hbase', 'table': 'document_demo', 'row_key': '20150527', 'fam': 'f1', 'col': 'c1'}

File Browser

    {'type': 'hdfs', 'path': '/data/hue/file.txt'}

Table Catalog

    {'type': 'hive', 'database': 'default', 'table': 'sample_07'}

![Data Links](https://cdn.gethue.com/uploads/2015/08/search-link-1024x630.png)

### Saved queries

Current selected facets and filters, query strings can be saved with a name within the dashboard. These are useful for defining “cohorts” or pre-selection of records and quickly reloading them.

![Rolling time](https://cdn.gethue.com/uploads/2015/08/search-query-def-1024x507.png)

### ‘Fixed’ or ‘rolling’ window

Real time indexing can now shine with the rolling window filter and the automatic refresh of the dashboard every N seconds. See it in action in the real time Twitter indexing with Spark streaming post.

![Fixed time](https://cdn.gethue.com/uploads/2015/08/search-fixed-time.png)

### 'More like this'

This feature lets you selected fields you would like to use to find similar records. This is a great way to find similar issues, customers, people... with regard to a list of attributes.

![More like this](https://cdn.gethue.com/uploads/2018/01/solr_more_like_this.png)


## Notebook

The goal of Notebooks is to quickly experiment with small programming snippets (with in particular Spark) and do interactive demos. Its goal is to stay lightweight with regards to other notebook or programming systems.

The main advantage is to be able to add snippets of different dialects (e.g. PySpark, Hive SQL...) into a single page:

![Notebook mode](https://cdn.gethue.com/uploads/2015/10/notebook-october.png)

Any configured language of the Editor will be available as a dialect. Each snippet has a code editor, wih autocomplete, syntax highlighting and other feature like shortcut links to HDFS paths and Hive tables have been added.

![Notebook Screen](https://cdn.gethue.com/uploads/2015/08/notebook.png)

Example of SparkR shell with inline plot

![Notebook r snippet](https://cdn.gethue.com/uploads/2015/08/spark-r-snippet.png)

All the spark-submit, spark-shell, pyspark, sparkR properties of jobs & shells can be added to the sessions of a Notebook. This will for example let you add files, modules and tweak the memory and number of executors.

![Notebook sessions](https://cdn.gethue.com/uploads/2015/08/notebook-sessions.png)

### Spark

Hue relies on [Livy](http://livy.io/) for the interactive Scala, Python, SparkSQL and R snippets.

Livy is an open source REST interface for interacting with Apache Spark from anywhere. It got initially developed in the Hue project but got a lot of traction and was moved to its own project on livy.io.

Make sure that the Notebook and interpreters [configured](/administrator/configuration/connectors/#apache-spark).

#### Livy

Starting the Livy REST server is detailed on livy.io the [get started](http://livy.incubator.apache.org/get-started/).

**Executing some Spark**

As the REST server is running, we can communicate with it. We are on the same machine so will use ‘localhost’ as the address of Livy.

Let’s list our open sessions

    curl localhost:8998/sessions

    {"from":0,"total":0,"sessions":[]}

**Note** You can use

    | python -m json.tool

at the end of the command to prettify the output, e.g.:

    curl localhost:8998/sessions/0 | python -m json.tool

There is zero session. We create an interactive PySpark session

    curl -X POST --data '{"kind": "pyspark"}' -H "Content-Type: application/json" localhost:8998/sessions

    {"id":0,"state":"starting","kind":"pyspark","log":[]}

Sessions ids are incrementing numbers starting from 0. We can then reference the session later by its id.

We check the status of the session until its state becomes idle: it means it is ready to be execute snippet of PySpark:

    curl localhost:8998/sessions/0 | python -m json.tool


    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current

                                    Dload  Upload   Total   Spent    Left  Speed

    100  1185    0  1185    0     0  72712      0 --:--:-- --:--:-- --:--:-- 79000

    {

        "id": 5,

        "kind": "pyspark",

        "log": [

          "15/09/03 17:44:14 INFO util.Utils: Successfully started service 'SparkUI' on port 4040.",

          "15/09/03 17:44:14 INFO ui.SparkUI: Started SparkUI at http://172.21.2.198:4040",

          "15/09/03 17:44:14 INFO spark.SparkContext: Added JAR file:/home/romain/projects/hue/apps/spark/java-lib/livy-assembly.jar at http://172.21.2.198:33590/jars/livy-assembly.jar with timestamp 1441327454666",

          "15/09/03 17:44:14 WARN metrics.MetricsSystem: Using default name DAGScheduler for source because spark.app.id is not set.",

          "15/09/03 17:44:14 INFO executor.Executor: Starting executor ID driver on host localhost",

          "15/09/03 17:44:14 INFO util.Utils: Successfully started service 'org.apache.spark.network.netty.NettyBlockTransferService' on port 54584.",

          "15/09/03 17:44:14 INFO netty.NettyBlockTransferService: Server created on 54584",

          "15/09/03 17:44:14 INFO storage.BlockManagerMaster: Trying to register BlockManager",

          "15/09/03 17:44:14 INFO storage.BlockManagerMasterEndpoint: Registering block manager localhost:54584 with 530.3 MB RAM, BlockManagerId(driver, localhost, 54584)",

          "15/09/03 17:44:15 INFO storage.BlockManagerMaster: Registered BlockManager"

        ],

        "state": "idle"

    }

![Livy Architecture sessions](https://cdn.gethue.com/uploads/2015/09/20150818_scalabythebay.024.png)

**Session properties**

All the properties supported by spark shells like the number of executors, the memory, etc can be changed at session creation. Their format is the same as when typing spark-shell -h

    curl -X POST --data '{"kind": "pyspark", "numExecutors": "3", "executorMemory": "2G"}' -H "Content-Type: application/json" localhost:8998/sessions
    {"id":0,"state":"starting","kind":"pyspark","numExecutors":"3","executorMemory":"2G","log":[]}

**Executing statements**

In YARN mode, Livy creates a remote Spark Shell in the cluster that can be accessed easily with REST

When the session state is idle, it means it is ready to accept statements! Lets compute 1 + 1

    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"1 + 1"}'

    {"id":0,"state":"running","output":null}

We check the result of statement 0 when its state is available

    curl localhost:8998/sessions/0/statements/0

    {"id":0,"state":"available","output":{"status":"ok","execution_count":0,"data":{"text/plain":"2"}}}

**Note** If the statement is taking less than a few milliseconds, Livy returns the response directly in the response of the POST command.

Statements are incrementing and all share the same context, so we can have a sequences

    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"a = 10"}'

    {"id":1,"state":"available","output":{"status":"ok","execution_count":1,"data":{"text/plain":""}}}

Spanning multiple statements

    curl localhost:8998/sessions/5/statements -X POST -H 'Content-Type: application/json' -d '{"code":"a + 1"}'

    {"id":2,"state":"available","output":{"status":"ok","execution_count":2,"data":{"text/plain":"11"}}}

Let’s close the session to free up the cluster. Note that Livy will automatically inactive idle sessions after 1 hour (configurable).

    curl localhost:8998/sessions/0 -X DELETE

    {"msg":"deleted"}

#### Tutorial: Sharing RDDs

This section shows how to share Spark RDDs and contexts. Livy offers remote Spark sessions to users. They usually have one each (or one by Notebook):

    # Client 1
    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"1 + 1"}'
    # Client 2
    curl localhost:8998/sessions/1/statements -X POST -H 'Content-Type: application/json' -d '{"code":"..."}'
    # Client 3
    curl localhost:8998/sessions/2/statements -X POST -H 'Content-Type: application/json' -d '{"code":"..."}'
    livy_shared_contexts2

![Livy shared context](https://cdn.gethue.com/uploads/2015/10/livy_shared_contexts2.png)

##### ... and so sharing RDDs

If the users were pointing to the same session, they would interact with the same Spark context. This context would itself manages several RDDs. Users simply need to use the same session id, e.g. 0, and issue commands there:

    # Client 1
    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"1 + 1"}'

    # Client 2
    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"..."}'

    # Client 3
    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"..."}'

![Livy multi rdds](https://cdn.gethue.com/uploads/2015/10/livy_multi_rdds2.png)

##### ...Accessing them from anywhere

Now we can even make it more sophisticated while keeping it simple. Imagine we want to simulate a shared in memory key/value store. One user can start a named RDD on a remote Livy PySpark session and anybody could access it.

![Livy anywhere rdds](https://cdn.gethue.com/uploads/2015/10/livy_shared_rdds_anywhere2.png)

To make it prettier, we can wrap it in a few lines of Python and call it ShareableRdd. Then users can directly connect to the session and set or retrieve values.

    class ShareableRdd():

    def __init__(self):
      self.data = sc.parallelize([])

    def get(self, key):
      return self.data.filter(lambda row: row[0] == key).take(1)

    def set(self, key, value):
      new_key = sc.parallelize([[key, value]])
      self.data = self.data.union(new_key)

set() adds a value to the shared RDD, while get() retrieves it.

    srdd = ShareableRdd()

    srdd.set('ak', 'Alaska')
    srdd.set('ca', 'California')

    srdd.get('ak')

If using the REST Api directly someone can access it with just these commands:

    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d '{"code":"srdd.get(\"ak\")"}'
    {"id":3,"state":"running","output":null}

    curl localhost:8998/sessions/0/statements/3
    {"id":3,"state":"available","output":{"status":"ok","execution_count":3,"data":{"text/plain":"[['ak', 'Alaska']]"}}}

We can even get prettier data back, directly in json format by adding the %json magic keyword:

    curl localhost:8998/sessions/0/statements -X POST -H 'Content-Type: application/json' -d  '{"code":"data = srdd.get(\"ak\")\n%json data"}'
    {"id":4,"state":"running","output":null}

    curl localhost:8998/sessions/0/statements/4
    {"id":4,"state":"available","output":{"status":"ok","execution_count":2,"data":{"application/json":[["ak","Alaska"]]}}}

##### Even from any languages

As Livy is providing a simple REST Api, we can quickly implement a little wrapper around it to offer the shared RDD functionality in any languages. Let’s do it with regular Python:

    pip install requests
    python

Then in the Python shell just declare the wrapper:

    import requests
    import json

    class SharedRdd():
      """
      Perform REST calls to a remote PySpark shell containing a Shared named RDD.
      """
      def __init__(self, session_url, name):
        self.session_url = session_url
        self.name = name

      def get(self, key):
        return self._curl('%(rdd)s.get("%(key)s")' % {'rdd': self.name, 'key': key})

      def set(self, key, value):
        return self._curl('%(rdd)s.set("%(key)s", "%(value)s")' % {'rdd': self.name, 'key': key, 'value': value})

      def _curl(self, code):
        statements_url = self.session_url + '/statements'
        data = {'code': code}
        r = requests.post(statements_url, data=json.dumps(data), headers={'Content-Type': 'application/json'})
        resp = r.json()
        statement_id = str(resp['id'])
        while resp['state'] == 'running':
          r = requests.get(statements_url + '/' + statement_id)
          resp = r.json()
        return r.json()['data']

Instantiate it and make it point to a live session that contains a ShareableRdd:

    states = SharedRdd('http://localhost:8998/sessions/0', 'states')

And just interact with the RDD transparently:

    states.get('ak')
    states.set('hi', 'Hawaii')