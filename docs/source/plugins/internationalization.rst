Internationalization
====================

Plugins usually let the user interact with Freedomotic, e.g. by showing
dialog boxes, logging messages and so on. I'd be advisable to let plugin
use the user's language in order to make them much usable and
understendable. If you're a developer and want to add your plugin the
capability to 'speak' different languages, just follow this simple guide
to adapt your code.

Adding internationalization support
-----------------------------------

Freedomotic ships a mechanism for easily support localized strings and
allows the developer to use a prebuild bag of general purpose strings.
Moreover the developer could add custom messages on his/her own.

First of all we need to access to API so let's add the following code

.. code:: 

    private API api;
    private I18n i18n;
    
    api = getApi();
    i18n = api.getI18n();
    
The static function to use in place of your 'unlocalized' string is ``i18n.msg(_STRING_KEY_)``.

Here a quick example

::

     // old code (non localized)
     LOG.info("Hello");
     // new code (localized)
     LOG.info(i18n.msg("greeting"));

In the plugin manifest file you have to add the property ``<property name="enable-i18n" value="true"/>``.


Behind the scenes - what happens when calling i18n.msg()?
---------------------------------------------------------

Freedomotic reads some system config to automatically guess user locale
it searches proper localization string inside ``/i18n/<Freedomotic locale>.properties``.

If the current locale is not defined (translation doesn't exist) **en\_UK**
is used and *Freedomotic.properties* file is loaded.

Making custom plugin translations
---------------------------------

If global translation strings aren't enough plugin developer could
write custom strings and save them using the following path starting
from plugin base folder

``/i18n/<package-name>.properties\`` for **en\_UK**

or

``/<plugin_package_name>/i18n/<package_last_part>-<locale>.properties``

e.g.

``/jfrontend/i18n/jfrontend.properties``

``/jfrontend/i18n/jfrontend-es_ES.properties``

Accessing custom plugin translations
------------------------------------

Just pass the current object as a parameter to ``i18n.msg()``

.. code:: java

   LOG.info("Plugin " + i18n.msg(this,"plug_name"));

Composing strings
-----------------

Consider the following example: we want to translate "**save environment
as**", "**save object as**", "**save room as**" and so on. The translation file
looks like this

+---------------+-----------------------+-----------------------+
| Key string    | Default translation   | it\_IT localization   |
+===============+=======================+=======================+
| save\_as      | Save as               | Salva come            |
+---------------+-----------------------+-----------------------+
| environment   | Environment           | Ambiente              |
+---------------+-----------------------+-----------------------+

using a concatenation of strings ``i18n.msg("save_as") + i18n.msg("environment");`` doesn't work and it'll result in "**save as environment**" ...

We can then use basic Java string format like that

+---------------+-----------------------+-----------------------+
| Key string    | Default translation   | it\_IT localization   |
+===============+=======================+=======================+
| save\_as      | Save {0} as...        | Salva {0} come...     |
+---------------+-----------------------+-----------------------+
| environment   | Environment           | Ambiente              |
+---------------+-----------------------+-----------------------+

.. code:: java

   i18n.msg("save_as",new Object[]{i18n.msg(environment)});

So the second variable is given as replacement for placeholder **{0}**.

This applies to many placeholders, not only one.

