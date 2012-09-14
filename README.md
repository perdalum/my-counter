# my-counter

A minimal application to test how simple a simple Hadoop job can be
written in Clojure. Actually rather simple, as can be seen.

The application is build upon the wordcount5.clj example from the
[clojure-hadoop](https://github.com/alexott/clojure-hadoop) distribution.

The application counts the frequencies of every work in a given text file.

## Usage

To build the job:

    $ lein deps, uberjar

To run the job local with out a Hadoop cluster:

    $ java -cp target/my-counter-0.1.0-SNAPSHOT-standalone.jar clojure_hadoop.job -job my-counter.core/job  -input ~/Desktop/shakespeare.txt -output out

where `~/Desktop/shakespeare.txt` is a plain text file and `out` will become a
directory. As the job definition has the `:replace` parameter set to `true`,
any pre-existing `out` directory will be replaced.

To run the job on a Hadoop cluster, you first need to import a text file to
HDFS by

    $ hadoop fs -mkdir in
    $ hadoop fs -copyFromLocal shakespeare.txt in

You can then execute the hadoop command

    $ hadoop jar my-counter-0.1.0-SNAPSHOT-standalone.jar clojure_hadoop.job -job my-counter.core/job -input in/shakespeare.txt -output out

and voilá

    $ hadoop fs -tail out/part-r-00000

shows you the frequency count. Well, the last 10 lines of it.

## TODO

Minimise the size of the *lein uberjar* JAR file. Only include what is nessecary
and rely on the class path on the Hadoop cluster.

## License

Copyright © 2012 Per Møldrup-Dalum

Distributed under the Eclipse Public License, the same as Clojure.
