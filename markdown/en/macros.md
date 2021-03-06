[Prev](plugins.md) ![Icinga](../images/logofullsize.png "Icinga") [Next](macrolist.md)

* * * * *

5.2. Understanding Macros and How They Work
-------------------------------------------

5.2.1. [Macros](macros.md#introduction)


5.2.3. [Example 1: Host Address Macro](macros.md#hostaddressexample)

5.2.4. [Example 2: Command Argument
Macros](macros.md#commandargexample)

5.2.5. [On-Demand Macros](macros.md#ondemand)

5.2.6. [On-Demand Group Macros](macros.md#ondemandgroup)

5.2.7. [Custom Variable Macros](macros.md#customvar)

5.2.8. [Macro Cleaning](macros.md#cleaning)

5.2.9. [Macros as Environment Variables](macros.md#environmentvars)

5.2.10. [Available Macros](macros.md#availablelist)

### 5.2.1. Macros

One of the main features that make Icinga so flexible is the ability to
use macros in command definitions. Macros allow you to reference
information from hosts, services, and other sources in your commands.


Before Icinga executes a command, it will replace any macros it finds in
the command definition with their corresponding values. This macro
host and service checks, notifications, event handlers, etc.

Certain macros may themselves contain other macros. These include the
\$HOSTNOTES\$, \$HOSTNOTESURL\$, \$HOSTACTIONURL\$, \$SERVICENOTES\$,
\$SERVICENOTESURL\$, and \$SERVICEACTIONURL\$ macros.

### 5.2.3. Example 1: Host Address Macro

When you use host and service macros in command definitions, they refer
to values for the host or service for which the command is being run.
Let's try an example. Assuming we are using a host definition and a
*check\_ping* command defined like this:

<pre><code>
 define host{
 define command{
</code></pre>

the expanded/final command line to be executed for the host's check
command would look like this:

</code></pre> 
$> /usr/local/icinga/libexec/check_ping -H 192.168.1.2 -w 100.0,25% -c 200.0,50%
</code></pre>

Pretty simple, right? The beauty in this is that you can use a single
command definition to check an unlimited number of hosts. Each host can
be checked with the same command definition because each host's address
is automatically substituted in the command line before execution.

### 5.2.4. Example 2: Command Argument Macros

You can pass arguments to commands as well, which is quite handy if
you'd like to keep your command definitions rather generic. Arguments
are specified in the object (i.e. host or service) definition, by
separating them from the command name with exclamation marks (!) like
so:

<pre><code>
 define service{
</code></pre>

In the example above, the service check command has two arguments (which
can be referenced with [\$ARGn\$](macrolist.md#macrolist-arg) macros).
The \$ARG1\$ macro will be "200.0,25%" and \$ARG2\$ will be "400.0,50%"
(both without quotes). Assuming we are using the host definition given
earlier and a *check\_ping* command defined like this:

<pre><code>
 define command{
</code></pre>

the expanded/final command line to be executed for the service's check
command would look like this:

</code></pre> 
$> /usr/local/icinga/libexec/check_ping -H 192.168.1.2 -w 200.0,25% -c 400.0,50%
</code></pre>

![[Tip]](../images/tip.png)

Tip

If you need to pass bang (!) characters in your command arguments, you
can do so by escaping them with a backslash (\\). If you need to include
backslashes in your command arguments, they should also be escaped with
a backslash.

![[Tip]](../images/tip.png)

Tip

If you need to pass a semicolon (;) character in your command arguments,
you cannot write the character in the definition because it is handled
as the start of a comment. You define a \$USER\$-variable in the
resource file instead and use that variable in your definition.

Resource file

<pre><code>
$USER255$=;
</code></pre>

Command definition

<pre><code>
}
</code></pre>

### 5.2.5. On-Demand Macros

Normally when you use host and service macros in command definitions,
they refer to values for the host or service for which the command is
being run. For instance, if a host check command is being executed for a
host named "linuxbox", all the [standard host
macros](macrolist.md "5.3. Standard Macros in Icinga") will refer to
values for that host ("linuxbox").

If you would like to reference values for another host or service in a
command (for which the command is not being run), you can use what are
called "on-demand" macros. On-demand macros look like normal macros,
except for the fact that they contain an identifier for the host or
service from which they should get their value. Here's the basic format
for on-demand macros:



Replace *HOSTMACRONAME* and *SERVICEMACRONAME* with the name of one of
the standard host of service macros found
[here](macrolist.md "5.3. Standard Macros in Icinga").

Note that the macro name is separated from the host or service
identifier by a colon (:). For on-demand service macros, the service
these are separated by a colon (:) as well.

![[Tip]](../images/tip.png)

Tip

On-demand service macros can contain an empty host name field. In this
case the name of the host associated with the service will automatically
be used.

Examples of on-demand host and service macros follow:

</code></pre> 
</code></pre>

On-demand macros are also available for hostgroup, servicegroup,
contact, and contactgroup macros. For example:

</code></pre> 
</code></pre>

### 5.2.6. On-Demand Group Macros

You can obtain the values of a macro across all contacts, hosts, or
services in a specific group by using a special format for your
on-demand macro declaration. You do this by referencing a specific host
group, service group, or contact group name in an on-demand macro, like
so:




Replace *HOSTMACRONAME*, *SERVICEMACRONAME*, and *CONTACTMACRONAME* with
the name of one of the standard host, service, or contact macros found
[here](macrolist.md "5.3. Standard Macros in Icinga"). The delimiter
you specify is used to separate macro values for each group member.

For example, the following macro will return a comma-separated list of
host state ids for hosts that are members of the *hg1* hostgroup:

</code></pre> 
 $HOSTSTATEID:hg1:,$
</code></pre>

This macro definition will return something that looks like this:

</code></pre> 
 0,2,1,1,0,0,2
</code></pre>

### 5.2.7. Custom Variable Macros

Any [custom object
variables](customobjectvars.md "3.5. Custom Object Variables") that
you define in host, service, or contact definitions are also available
as macros. Custom variable macros are named as follows:




Take the following host definition with a custom variable called
"\_MACADDRESS"...

<pre><code>
 define host{
</code></pre>

The \_MACADDRESS custom variable would be available in a macro called
\$\_HOSTMACADDRESS\$. More information on custom object variables and
how they can be used in macros can be found
[here](customobjectvars.md "3.5. Custom Object Variables").

### 5.2.8. Macro Cleaning

Some macros are stripped of potentially dangerous shell metacharacters
before being substituted into commands to be executed. Which characters
are stripped from the macros depends on the setting of the
[illegal\_macro\_output\_chars](configmain.md#configmain-illegal_macro_output_chars)
directive. The following macros are stripped of potentially dangerous
characters:










10. [\$SERVICEACKCOMMENT\$](macrolist.md#macrolist-serviceackcomment)

### 5.2.9. Macros as Environment Variables

Most macros are made available as environment variables for easy
reference by scripts or commands that are executed by Icinga. For
purposes of security and sanity,
[\$USERn\$](macrolist.md#macrolist-user) and "on-demand" host and
service macros are not made available as environment variables.

Environment variables that contain standard macros are named the same as
their corresponding macro names (listed
[here](macrolist.md "5.3. Standard Macros in Icinga")), with
"ICINGA\_" prepended to their names. For example, the
[\$HOSTNAME\$](macrolist.md#macrolist-hostname) macro would be
available as an environment variable named "ICINGA\_HOSTNAME".

### 5.2.10. Available Macros

A list of all the macros that are available in Icinga, as well as a
chart of when they can be used, can be found
[here](macrolist.md "5.3. Standard Macros in Icinga").

* * * * *

[Prev](plugins.md) | [Up](ch05.md) | [Next](macrolist.md)

5.1. Icinga Plugins  |<=== [Index](index.md) ===>|  5.3. Standard Macros in Icinga

© 1999-2009 Ethan Galstad, 2009-2015 Icinga Development Team,
http://www.icinga.org
