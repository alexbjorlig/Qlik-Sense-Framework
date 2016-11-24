.. _themes:

Themes
======

.. danger:: You are about to use experimental features of Qlik Sense. Backup your files!

Colors in Qlik Sense
--------------------

**Are looking for a way to change the look and fell of your Qlik Sense applications?**

Qlik Sense does currently not officially support color shceme modification, but there is
an experimental feature that lets you test the capability.
To understand how this work, think of Qlik Sense as a web server and the client accessing documents
through the browser:

.. figure:: images/themes/color_scheme/iqsf_color_scheme_1.PNG
  :scale: 80%

Instead of using Qlik Sense to browse an application, you can use Google Chrome to open a Qlik Sense app.
Make sure that Qlik Sense is running and browse this `URL <http://localhost:4848/hub/>`_:

.. code-block:: html

  localhost:4848/hub

This opens your hub utilizing the browser. If you extend the URL with */sense/app* Qlik
Sense will look for a specific app. To open the the default Executive Dashboard copy this in your browser,
and make sure to substitute your username:

.. code-block:: html

  http://localhost:4848/sense/app/C%3A%5CUsers%5C<user>%5CDocuments%5CQlik%5CSense%5CApps%5CExecutive%20Dashboard/

This should open the app. Finally try to specify a specific sheet. Remember to substitute your username.

.. code-block:: html

  http://localhost:4848/sense/app/C%3A%5CUsers%5C<user>%5CDocuments%5CQlik%5CSense%5CApps%5CExecutive%20Dashboard/sheet/PfKsJK/state/analysis

With the sheet open in Google Chrome, activate the developer tools by pressing: Ctrl + Shift + I.
Try to change the background-color of the sheet to red:

.. figure:: images/themes/color_scheme/iqsf_color_scheme_2.PNG
  :scale: 80%

This should give you a good idea of how Qlik Sense works, and how/why we can change the
color scheme. Now lets continue to look at how this can be implemented.

Create a Color Scheme
*********************

To create a custom color scheme you can use the color scheme generator:

.. code-block:: html

  http://qlik-sense-colors.herokuapp.com


The output of the generator needs to be copied to the theme.json file.



Change Color Scheme: temporary
******************************

Before changing the color scheme on our local machine, we first need a new color scheme.
You can download a color theme from the Clik Community called `highvis <https://community.qlik.com/docs/DOC-13517>`_.
After downloading and extracting the zip file, you copy the folder to the Qlik Sense themes folder:

The default location of the folder is

.. code-block:: html

  C:\Program Files\Qlik\Sense\Client\themes

If you do not know the location of Qlik Sense you can right click the program icon in the
windows menu bar and choose Properties. Under Shortcut\Target the path to Qlik Sense is listed.

.. figure:: images/themes/color_scheme/iqsf_color_scheme_3.PNG
  :scale: 60%

After copying the file and if Qlik Sense is open, you can now open the default app
with a different theme by setting the parameter to highvis.

.. code-block:: html

  http://localhost:4848/sense/app/C%3A%5CUsers%5C<User>%5CDocuments%5CQlik%5CSense%5CApps%5CExecutive%20Dashboard/sheet/PfKsJK/state/analysis/theme/highvis

Change Color Scheme: permanent
******************************

.. danger:: You are thinking about changing system files. Read the documentation and backup your files!

To change the color scheme permanent, you have to change the default theme.

After you have done this, you have to change a javascript file called require.js
This is a JavaScript file, with the responsibility of loading modules. You can read more about it `here <http://requirejs.org/>`__.
The default path is:

.. code-block:: html

  C:\Program Files\Qlik\Sense\Client\assets\external\requirejs\

Open a text editor and search for

.. code-block:: javascript

  define("text!

Now change the end of file accordingly:

.. code-block:: javascript

  function(a,b){function c(b)} // lots of code
  onBlockRender:function(){th  // lots of code
  //,define("text!themes/sen   // lots of code
  ;

The line we have commented out *//,define("text!themes...* declares the default theme inline. By removing the line,
Qlik Sense will now read the default theme, and you can change it accordingly. If you get problems by modifying the
require.js file please see the chapter on debuggning using the browser.

.. note:: The change will first occur when restarting Qlik Sense Proxy Service (QPS). You can read more about
  Qlik Sense services `here <https://help.qlik.com/en-US/sense/1.0/Subsystems/Server_deployment_and_configuration/Content/Qlik%20Sense/QSSRM_Architecture_Concepts_Services_Proxy.htm>`_.
  You stop/start this service using the windows services program.

Missing Features and todos
**************************

* Organizing custom color themes i.e. assigning custmo themes to specific applications.
  In a comment to this `Qlik Community post <https://community.qlik.com/docs/DOC-13517>`_ there is mentioned that *something is on the roadmap for 3.0+*.
* Designing color themes. A reasonable work around is to use the `atom <https://atom.io/>`_ text editor with two packages installed: `color-picker <https://atom.io/packages/color-picker>`_ and `pigments <https://github.com/abe33/atom-pigments>`_.
  Consider reading the atom flight `manual <http://flight-manual.atom.io/>`_.
  It gives you the possibility of viewing and setting colors in file:

  .. figure:: images/themes/color_scheme/iqsf_color_scheme_atom.PNG
    :scale: 50%

* Understanding the role of the css file located with the theme. It is for example responsible for changes to sheet background color.
* The creation of theme github repository to share custom color themes

Icons
-----
As a result of the web structure of Qlik Sense, it is possible to change the icons as well.
The following example will demonstrate how to change the icons in the hub.
This guide is a summery of the this `blog post <http://livingqlikview.com/how-to-qlik-sense-branding-2-white-label-the-hub/>`_.

Create the Icons
****************
The first step is to create the desired icons, using your preferred tool.
The following icons are needed, with the exact naming and size:

+----------------------+---------------+
| Name                 | Size          |
+======================+===============+
| favicon.ico          | 64x64 pixels  |
+----------------------+---------------+
| logo-60x60.png       | 60x60 pixels  |
+----------------------+---------------+
| logo-76x76.png       | 76x76 pixels  |
+----------------------+---------------+
| logo-120x120.png     | 120x120 pixels|
+----------------------+---------------+
| logo-152x152.png     | 152x152 pixels|
+----------------------+---------------+
| qlik-sense-server.png| 548x79 pixels |
+----------------------+---------------+
| qlik-sense.png       | 346x66 pixels |
+----------------------+---------------+

Copy the icons to the server
****************************

On the Qlik Sense server, replace the favicon at the following path:

.. code-block:: html

  C:\Program Files\Qlik\Sense\Client\

Next, replace the icons at the following path:

.. code-block:: html

  C:\Program Files\Qlik\Sense\Client\img\core\logo

.. note:: The change will first occur when restarting Qlik Sense Proxy Service (QPS), and the Qlik Sense Engine Service. You can read more about
  Qlik Sense services `here <https://help.qlik.com/en-US/sense/1.0/Subsystems/Server_deployment_and_configuration/Content/Qlik%20Sense/QSSRM_Architecture_Concepts_Services.htm>`_.
  You stop/start the services using the windows services program.


Fonts
-----
The Qlik Sense logic of managing colors is identical to how fonds are managed.

Change Font Size: temporary
***************************

As a result of many areas with text in Qlik Sense, the size can be changed for all
objects. If you collapse the theme.json file (use the line numbers for reference), you can see that
all the different types of objects in Qlik Sense are present:

.. figure:: images/themes/fonts/objects_in_theme.PNG
  :scale: 80%

All of theese objects have a one or more settings for the fontsize. Lets say that your objective is to change
the font size in the bar chart. When you inspect the properties you should find something similar to this:

.. code-block:: javascript

  "fontSize": {
              "default": "11px",
              "full": "11px",
              "medium": "11px",
              "small": "11px",
              "xsmall": "11px",
              "spark": "11px"
          }

Now change the full pixel size to 21px, and go to a browser and open the following. Make sure Qlik Sense is running
and change your username accordingly:

.. code-block:: html

  http://localhost:4848/sense/app/C%3A%5CUsers%5C<user>%5CDocuments%5CQlik%5CSense%5CApps%5CExecutive%20Dashboard/sheet/FaQeFa/state/analysis/theme/sense

Now you should hopefully see the bar chart with increased font size:

.. figure:: images/themes/fonts/increased_font_size.PNG
  :scale: 60%

Change Font Size: permanent
***************************

To change to font size permanently follow the same instructions as for how to change the Color Theme.

.. note:: The change will first occur when restarting Qlik Sense Proxy Service (QPS). You can read more about
  Qlik Sense services `here <https://help.qlik.com/en-US/sense/1.0/Subsystems/Server_deployment_and_configuration/Content/Qlik%20Sense/QSSRM_Architecture_Concepts_Services_Proxy.htm>`_.
  You stop/start this service using the windows services program.

Missing Features and todos
**************************

* Organizing custom fonts and assigning custom fonts to specific applications.
* Understanding the role of the css file located with the theme. It is for example responsible for changes to some sheet font sizes.
* A way of changing the font size using variables so the "baseline" font size can be increased effortless
