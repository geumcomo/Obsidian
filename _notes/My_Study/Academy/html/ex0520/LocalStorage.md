```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #results{
            border-top: 2px solid #ababab;

        }
        header>h1{
            
            border-bottom: 2px solid #ababab;
        }
    </style>
</head>
<body>
    <div id = "content">
        <header>
            <h1>Local Storage</h1>
        </header>
        <div id = "form">
            <form id ="travelForm" >
                <table class = "form">
                    <tbody>
                        <tr>
                            <td class = "label">Traveler</td>
                            <td>
                                <input type="text" name="traveler" id="traveler"/>
                            </td>
                        </tr>
                        <tr>
                            <td class = "label">Destination</td>
                            <td>
                                <input type="text" name="destination" id="destination"/>
                            </td>
                        </tr>
                        <tr>
                            <td class = "label">Transportation</td>
                            <td>
                                <input type="text" name="transportation" id="transportation"/>
                            </td>
                        </tr>
                    </tbody>
                    <tfoot>
                        <tr>
                            <td colspan="2" class = "button">
                                <button type="button" onclick="addDate()">save</button>
                                <button type="button" onclick="clearStorage()">clear</button>
                            </td>
                        </tr>
                    </tfoot>
                </table>
            </form>
        </div>
        <div id="results">
            <!-- 저장된 storage 값을 표현하는 곳  -->

        </div>
    </div>
<script>
    let db = getStorage(); //먼저 실행됨

    // 현재 창이 열리고 읽혀지면 무조건 수행하는 이벤트
    window.onload = function(){
        res = document.getElementById("results");
        init();
    }
    function getStorage(){
        try {
            //현재 브라우저가 localStorage를 사용할 수 있는지?
            // 검증한 후 저장소를 반환
            if(window.localStorage) //변수에 0과 1이 들어가면 true
            return window.localStorage; //db로 넣어준다.
        } catch (e) {
            return undefined;
        }
    }
    function init(){
        // 만약! 저장소(db)에 저장된 값이 있을 때 result라는 
        //아이디를 가진 div에 표현하는 함수

        // 결과 값 초기화
        let result = "";
        
        //db에 저장된 key와 value를 얻어내어
        
        //result 변수에 적재한다.
        
        // session: 서버가 관리 cookie: 클라이언트가 관리
        
        for(let i=0; i<db.length; i++){
            // 저장소에 저장된 값들은 key와 value가 쌍으로 짝을 이뤄서
            //  저장됨! (자바의 Map과 같다.)
            let key = db.key(i); //키 얻어내기 eterator
            let value = db.getItem(key); 
            // 위에서 얻어낸 키를 이용하여 연결된 value를 얻어낸다.
            result += key+":"+ value+"<br/>";

        } //끝
        //현재문서내에 아이디가 results인 div인 요소에 html로 표현
        // console.log(result);
        res.innerHTML = result;
        
    }
    function addDate(){
        // 사용자가 입력한 값들 (traveler, destination,transportation)을 가져와야 한다.
        let v1 = document.getElementById("traveler").value;
        let v2 = document.getElementById("destination").value;
        let v3 = document.getElementById("transportation").value;
        //유효성 검사를 해야하지만 패쓰!
        // 저장소에 키와 값을 연결지어서 저장하면 됨
        db.setItem("traveler", v1);
        db.setItem("destination", v2);
        db.setItem("transportation", v3);

    }
    function clearStorage(){
        db.clear();
        // 원하는 키로 하나만 삭제
        // db.removeItem("키")
    }
    
</script>
</body>
</html>
```