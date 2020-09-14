
<!DOCTYPE md>
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta charset="utf-8" />
    <title>CodeSkulptor</title>
    <link href="favicon.ico" rel="shortcut icon" />
    <link rel="stylesheet" type="text/css" href="codemirror.css" />
    <link rel="stylesheet" type="text/css" href="jqui/jquery-ui-1.10.3.custom.min.css" />
    <link rel="stylesheet" type="text/css" href="codeskulptor.css" />
    <link rel="stylesheet" type="text/css" href="dialog.css" />
    <script type="text/javascript" src="js/codemirror-compressed.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.10.3/jquery-ui.min.js"></script>
    <script type="text/javascript" src="js/jquery.flot.min.js"></script>
    <script type="text/javascript" src="js/jquery.flot.axislabels.min.js"></script>
    <script type="text/javascript" src="js/jquery.flot.orderbars.min.js"></script>
    <script type="text/javascript" src="js/numeric-1.2.6.min.js"></script>
    <script type="text/javascript" src="skulpt/skulpt.min.js"></script>
    <script type="text/javascript" src="skulpt/skulpt-stdlib.js"></script>
    <script type="text/javascript" src="js/codeskulptor-compressed.js"></script>
    <script src="https://maps.googleapis.com/maps/api/js?v=3.exp&sensor=false"></script>

<script type="text/javascript">
  var _gaq = _gaq || [];
  _gaq.push(['_setAccount', 'UA-33662649-1']);
  _gaq.push(['_trackPageview']);

  (function() {
    var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
    ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
    var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
  })();
</script>
  </head>

  <body>
    <div id="main">
      <div id="advert">
      	Looking for Python 3?
      	Try <a href="http://py3.codeskulptor.org/">py3.codeskulptor.org</a>!
      </div>
      <div id="controls" class="topbar">
        <div style="float:left">
          <button type="button" id="run" class="tb-button-icon ui-state-default" accesskey="R">Run (Accesskey R)</button>
          <button type="button" id="save" class="tb-button-icon ui-state-default" accesskey="S">Save (Accesskey S)</button>
          <a id="dlhref" href=""></a>
          <button type="button" id="dl" class="tb-button-icon ui-state-default">Download</button>
          <button type="button" id="fresh" class="tb-button-icon ui-state-default">Fresh URL</button>
          <button type="button" id="loadlocal" class="tb-button-icon ui-state-default">Open Local<input type="file" id="localfile"></input></button>
          <button type="button" id="reset" class="tb-button-icon ui-state-default" accesskey="X">Reset (Accesskey X)</button>
        </div>
        <img id="brand" src="codeskulptor.png" alt="CodeSkulptor" />
        <div style="float:right">
          <a id="docanchor" href="docs.html" target="_blank"></a>
          <button type="button" id="docs" class="tb-button-text ui-state-default">Docs</button>
          <a id="demoanchor" href="demos.html" target="_blank"></a>
          <button type="button" id="demos" class="tb-button-text ui-state-default">Demos</button>
          <a id="tipanchor" href="viz" target="_blank"></a>
          <button type="button" id="tips" class="tb-button-text ui-state-default">Viz Mode</button>
        </div>
      </div>
      <div id="active">
        <div id="eddiv">
          <form action="" method="post" enctype="multipart/form-data" id="codeform">
            <input type="hidden" name="key" id="keyid" />
            <input type="hidden" name="Content-Type" value="text/x-python" />
            <input type="hidden" name="GoogleAccessId" id="googleid" />
            <input type="hidden" name="Policy" id="policy" />
            <input type="hidden" name="Signature" id="signature" />
            <textarea id="code" name="file"></textarea>
          </form>
        </div>
        <div id="splitbar">
          <div id="grip">
          <img src="jqui/images/ui-icons_00246a_256x240.png" />
          </div>
        </div>
        <div id="console"></div>
      </div>
      <div id="bottom">
        CodeSkulptor was built by <a href="http://www.cs.rice.edu/~rixner/">
        Scott Rixner</a> and is based
        upon <a href="http://codemirror.net">CodeMirror</a> and
        <a href="http://skulpt.org">Skulpt</a>.
      </div>
    </div>
  </body>
</html>
