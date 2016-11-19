Overview
========

It takes in input some text, analyzes it to compute a similarity value
related to a set of predefined objects available to the system (eg:
automation commands)

How to work
-----------

It makes a text analysis and ranks objects according to their similarity
value. These objects are typically
`Commands <https://github.com/freedomotic/freedomotic/wiki/Commands>`__,
for example a speech recognition algorithm returns a text and you want
to identify the most similar predefined command to execute it. Beware
that computing similarity may be CPU intensive. Similarity may be
computed with different algorithms. Our solution is based on
`Damerau-Levenshtein
distance <https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance>`__.
View our `implemented
class <https://github.com/freedomotic/freedomotic/blob/master/framework/freedomotic-core/src/main/java/com/freedomotic/nlp/NlpCommandStringDistanceImpl.java>`__.

How to use it
-------------

The software listens to free-form (natural language) text commands on
channel ``app.commands.interpreter.nlp`` and execute most similar
command that the framework has in memory. For example a speech
recognition utility may return a free-form text that can be interpreted
by this module as an executable command. Another example is a chat bot
that executes text commands.

Code sample
-----------

.. code:: java

    Command nlpCommand = new Command();
    nlpCommand.setName("Recognize text with NLP");
    nlpCommand.setReceiver("app.commands.interpreter.nlp");
    nlpCommand.setDescription("A free-form text command to be interpreded by an NLP module");
    nlpCommand.setProperty("text", text);
    nlpCommand.setReplyTimeout(10000);
    Command reply = send(nlpCommand);

A complete code sample can be found
`here <https://github.com/freedomotic/freedomotic/blob/master/plugins/devices/simulation/src/main/java/com/freedomotic/plugins/VariousSensors.java>`__.
