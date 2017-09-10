
.. image:: img/sultan-logo.png
    :alt: Sultan logo
    :align: center
-------------------------------

**Command and Rule Over Your Shell**

-------------------------------

======
Sultan
======
.. automodule:: sultan.api

.. toctree::
    :caption: Table of Contents
    :name: mastertoc
    :maxdepth: 2

    install-sultan
    faq
    sultan-examples
    sultan-ssh-examples

-------------------------------

---------------
What is Sultan?
---------------

Sultan is a Python package for interfacing with command-line utilities, like 
`yum`, `apt-get`, or `ls`, in a Pythonic manner. It lets you run command-line 
utilities using simple function calls. 

The simplest way to use Sultan is to just call it:

.. code:: python

  from sultan.api import Sultan
  s = Sultan()
  s.sudo("yum install -y tree").run()
  
**Runs:** 

.. code:: bash

  sudo yum install -y tree;

The recommended way of using Sultan is to use it in Context Management mode. 
Here is how to use Sultan with Context Management:

.. code:: python

  from sultan.api import Sultan

  with Sultan.load(sudo=True) as s:
    s.yum("install -y tree").run()

**Runs:** 

.. code:: bash
  
  sudo su - root -c 'yum install -y tree;'

What if we want to install this command on a remote machine? You can easily 
achieve this using context management:

.. code:: python

  from sultan.api import Sultan
  
  with Sultan.load(sudo=True, hostname="myserver.com") as sultan:
    sultan.yum("install -y tree").run()

**Runs:**

.. code:: bash

  ssh root@myserver.com 'sudo su - root -c 'yum install -y tree;''

If you enter a wrong command, Sultan will print out details you need to debug and 
find the problem quickly.

Here, the same command was run on a Mac:

.. code:: python

  from sultan.api import Sultan
  
  with Sultan.load(sudo=True, hostname="myserver.com") as sultan:
    sultan.yum("install -y tree").run()

  
**Yields:**

.. code:: bash

  [sultan]: sudo su - root -c 'yum install -y tree;'
  Password:
  [sultan]: --{ STDERR }-------------------------------------------------------------------------------------------------------
  [sultan]: | -sh: yum: command not found
  [sultan]: -------------------------------------------------------------------------------------------------------------------

Want to get started? Simply install Sultan, and start writing your clean code::

    pip install --upgrade sultan

If you have more questions, check the rest of the docs, or reach out at 
Github: https://github.com/aeroxis/sultan


WARNING * WARNING * WARNING
---------------------------

When you're using Sultan, you are running commands directly on your local shell,
so please, do not run untested and untrusted code. You are taking the risk if
you are running untrusted code. 

Sultan runs *POpen* with *shell=True*, and according to Python documentation,
this can be a security hazard if combined with untrusted input. More information
can be found here: 

* Python 2: https://docs.python.org/2/library/subprocess.html#frequently-used-arguments
* Python 3: https://docs.python.org/3/library/subprocess.html#frequently-used-arguments