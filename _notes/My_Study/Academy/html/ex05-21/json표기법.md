![[Pasted image 20240521102807.png]]
![[Pasted image 20240521102812.png]]
# modal.html

```html
<!DOCTYPE html><html lang="en"><head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>jQuery UI Dialog - Default functionality</title>
    <link rel="stylesheet" href="https://code.jquery.com/ui/1.13.3/themes/base/jquery-ui.css">
    
    <script src="https://code.jquery.com/jquery-3.7.1.js"></script>
    <script src="https://code.jquery.com/ui/1.13.3/jquery-ui.js"></script>
    <script>
    $( function() {
        let option = {
            modal: true, //앞에꺼가 키 뒤에 value (map구조) 키: value json표기법
            autoOpen: false, //호출되는 즉시 대화상자 표시 default는 true
            position:{my:"left top", at:"right bottom", of:"#bt3"},
            title: "SIST교육센터",
            resizable: true,
            // my: 자신의 위치(left, right, top, bottom, center)
            // at: 기점이 되는 요소상의 위치
            // of: 기점이 되는 요소(선택자로  지정)
        }; //옵션이 여러개면 {}
      $( "#dialog" ).dialog(option);
        $("button").button();
        $("button").click(function(){
            $("#dialog").dialog("open");
        });
        $("#btn").click(function(){
            $("#dialog").dialog("close");
        });

    } );
    </script>
  </head>
  <body>
   
  <div id="dialog" title="쌍용교육센터">
    <p>This is the default dialog which is useful for displaying information. The dialog window can be moved, resized and closed with the 'x' icon.</p>
    <button type="button" id="btn">닫기</button>
  </div>
  <button type ="button" id ="bt1" >버튼1</button>
  <button type ="button" id ="bt1">버튼2</button>
  <button type ="button" id ="bt1">버튼3</button>
  
   
   
   
   
  </body></html>
```