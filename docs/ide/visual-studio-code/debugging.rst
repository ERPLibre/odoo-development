============
 Debugging
============

Launch Configurations
---------------------

    To debug your app in VS Code, you'll first need to set up your launch configuration file - launch.json. 
    Click on the Configure gear icon on the Debug view top bar, choose your debug environment and VS Code will 
    generate a launch.json file under your workspace's .vscode folder.

**sample python Debugging**

.. code-block:: js

        {
            "name": "Python",
            "type": "python",
            "request": "launch",
            "stopOnEntry": false,
            "pythonPath": "${config:python.pythonPath}",
            //"program": "${file}", use this to debug opened file.
            "program": "${workspaceRoot}/Path/To/odoo.py",
            "args": [
              "-c ${workspaceRoot}/sampleconfigurationfile.cfg"  
            ],
            "cwd": "${workspaceRoot}",
            "console": "externalTerminal",
            "debugOptions": [
                "WaitOnAbnormalExit",
                "WaitOnNormalExit",
                "RedirectOutput"
            ]
        },  

.. important:: use "args" to specify any options like database, config or user name and password.  Refer to `Odoo documentation <https://www.odoo.com/documentation/13.0/reference/cmdline.html>`_ for more info.

**Another example - Update module for a database**

.. code-block:: js

        {
            "name": "Python: Update Dev",
            "type": "python",
            "request": "launch",
            "stopOnEntry": false,
            "pythonPath": "${command:python.interpreterPath}",
            "console": "integratedTerminal",
            "program": "${workspaceRoot}/odoo/odoo-bin",
            "args": [
                "--limit-time-real=99999",
                "--limit-time-cpu=9999",
                "--config=${workspaceRoot}/config.conf",
                "--stop-after-init",
                "--database=database_name",
                "--update=module_name",
            ],
            "cwd": "${workspaceRoot}",
            "env": {},
            "envFile": "${workspaceRoot}/.env",
        },

`source <https://code.visualstudio.com/Docs/editor/debugging>`_
