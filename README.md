uk.org.bobulous.java.startup
============================

Package which includes the <code>MainArgsHandler</code> utility class. Depends on both of the packages <a href="https://github.com/Bobulous/uk.org.bobulous.java.toolkit">uk.org.bobulous.java.toolkit</a> and <a href="https://github.com/Bobulous/uk.org.bobulous.java.intervals">uk.org.bobulous.java.intervals</a>.

<h2>MainArgsHandler</h2>

Processes command-line arguments supplied to a Java application's
<code>main</code> method.
<p>
Provides methods for specifying which flags and variables are permitted as
arguments to the main method of an application, and methods for processing
supplied arguments to check for compliance and retrieve variable values and
determine the presence of flags.</p>
<p>
To use <code>MainArgsHandler</code> follow these steps in order:</p>
<ol>
<li>Acquire the sole instance of <code>MainArgsHandler</code> by calling the
static method <code>getHandler</code>.</li>
<li>Specify which flag names, if any, are permitted in the command-line
arguments for your application by calling the
<code>permitFlag(String, String)</code> method for each flag.</li>
<li>Specify which variables, if any, are permitted in the command-line
arguments, and how many times they must or may appear, by calling the
<code>permitVariable(String, Interval, String)</code> for each variable.</li>
<li>Call the <code>processMainArgs(String[])</code> method to process the String
array received by your application's <code>main(String[])</code> method. Be
prepared for an <code>IllegalArgumentException</code> to be thrown if the
user calls your application with unrecognised command-line flags or variables
or incorrect quantities of permitted variables.</li>
<li>Check for the presence of a permitted flag in the command-line arguments
by calling <code>foundFlag(String)</code>.</li>
<li>Check for the presence of a permitted variable in the command-line
arguments by calling <code>foundVariable(String)</code>.</li>
<li>If a variable is found on the command-line, get a
<code>List&lt;String&gt;</code> of its values by calling
<code>getValuesFromVariable(String)</code> for that variable name.</li>
</ol>
<p>
Each command-line argument can be either a <dfn>flag</dfn> (which has no
value) or a <dfn>variable</dfn> (which does have a value).</p>
<p>
A command-line flag has the form <kbd>--flag-name</kbd> (two hyphens then the
flag-name) where the flag-name begins with lowercase a-z and can then contain
lowercase a-z sequences separated by single hyphens (consecutive hyphens
cannot appear within flag names, so <kbd>--flag--name</kbd> would be illegal
for instance).</p>
<p>
A command-line variable has the form <kbd>--variable-name=value</kbd> (two
hyphens then the variable-name then equals-sign then value) where the
variable-name must follow the same rules as a flag-name. The value can
contain any characters (though this can be constrained by specifying a
<code>Pattern</code> in the call to <code>permitVariable</code>). (Bear in
mind that to pass Java a value which contains spaces the command-line
argument must wrap the value in double-quotes, e.g.
<kbd>--window-title="Nightly Build v1.740"</kbd> and also note that the
double-quotes are automatically stripped away by Java before they reach the
main method.)</p>
<p>
Note that the initial double-hyphen is only used on the command-line, and
does not form part of the flag or variable name. The initial double-hyphen
must not be included when calling methods such as <code>permitFlag(String)</code>
or <code>getValuesFromVariable(String)</code>.</p>
<p>
If a flag or variable is encountered which does not fit the permitted syntax
then an <code>IllegalArgumentException</code> will be thrown. It is advised
to catch this exception and then present to the end-user an error message and
the content of the <code>String</code> returned by the method
<code>getUsageSummary()</code></p>
<p>
It is permitted to have a command-line call which provides the same variable
name more than once, to provide multiple values for that variable. For
instance the following command-line arguments are permitted in a single call:
<kbd>--path=/usr/bin --path=/usr/local/bin</kbd> and both of these values
will be stored in the map for this variable name "path".</p>
<p>
It is not possible to use the same name for both a flag and a variable.</p>
