# /TypeScript extended RegExps/x

Provides extended (and genuinely-multi-line) Regular Expressions using tagged template strings. 

For an explanation of extended RegExps, see http://blog.mackerron.com/2010/08/08/extended-multi-line-js-regexps/ (this is from 2010, when this was just for plain JavaScript).

Now available for TypeScript, where tagged template strings make this genuinely usable and useful. The general syntax is:

    xRegExp`myregexp`('flags');

In addition to the standard flags (`i`, `g`, etc.), which are simply passed through to the native `RegExp`, three additional flags are provided:

* `x` activates extended mode, stripping out whitespace and comments
* `mm` activates genuinely-multi-line mode, where `.` matches anything, including newlines (achieved by replacing `.` with `[\s\S]`)
* `\\` automatically escapes all template expressions so they are treated as literal text (alternatively, an `xRegExp.escape` method is provided so this can be done case-by-case).

An example with flag `x`:
    
    import xRegExp from 'xregexps';
    
    const codeRegExp = xRegExp`
      ^
      \s*
      ([0-9]+)      # code 1
      \.
      ([a-z]+)      # code 2
      \s*
      $
    `('xi');
    
    console.log(codeRegExp);  // -> /^\s*([0-9]+)\.([a-z]+)\s*$/i
    
An example with flag `\\`:

    import xRegExp from 'xregexps';
    
    const separator = '.';
    const codeRegExp = xRegExp`
      ^
      \s*
      ([0-9]+)      # code 1
      ${separator}
      ([a-z]+)      # code 2
      ${separator}
      ([0-9]+)      # code 3
      \s*
      $
    `('\\xi');
    
    console.log(codeRegExp);  // -> /^\s*([0-9]+)\.([a-z]+)\.([0-9]+)\s*$/i
    
The same example *without* flag `\\` (and importing the template function under a briefer name):

    import xRE from 'xregexps';
    
    const separator = '.';
    const codeRegExp = xRE`
      ^
      \s*
      ([0-9]+)      # code 1
      ${xRE.escape(separator)}
      ([a-z]+)      # code 2
      ${xRE.escape(separator)}
      ([0-9]+)      # code 3
      \s*
      $
    `('xi');
    
    console.log(codeRegExp);  // -> /^\s*([0-9]+)\.([a-z]+)\.([0-9]+)\s*$/i
    
    
