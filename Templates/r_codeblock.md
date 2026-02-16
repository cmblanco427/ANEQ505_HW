<%*
tR += "```r\n\n```";

setTimeout(() => {
  const editor = app.workspace.activeEditor.editor;
  const cursor = editor.getCursor();
  // Move cursor up one line to the empty middle line
  editor.setCursor(cursor.line - 2, 0);
}, 50);
%>
