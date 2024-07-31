<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>나만의 추억앨범</title>
    <!-- 제이쿼리 cdn -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <!-- 부트스트랩 cdn -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
        <!-- popperjs/core ui 요소의 위치를 관리하는 라이브러리. -->
        <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.6/dist/umd/popper.min.js"></script>
        <!-- Bootstrap의 자바스크립트 기반 컴포넌트를 포함하는 라이브러리 -->
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.min.js"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Do+Hyeon&display=swap');

        * {
            font-family: "Do Hyeon", sans-serif;
            font-style: normal;
        }

        .mytitle {
            height: 250px;
            color: white;

            /* 박스 안의 내용 가운데 정렬 */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;

            background-image: url('https://images.unsplash.com/photo-1511992243105-2992b3fd0410?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1470&q=80');
            background-position: center;
            background-size: cover;
        }

        .mytitle>button {
            width: 150px;
            height: 50px;
            /* 투명한 색 */
            background-color: transparent;
            color: white;
            font-size: 18px;
            font-weight: bold;

            border: 1px solid white;
            border-radius: 5px;

            margin-top: 20px;

        }

        .mycards {
            width: 1200px;
            margin: 20px auto 20px auto;
        }

        .mypostingbox {
            width: 500px;
            margin: 20px auto 20px auto;

            /* 안쪽으로 여백 */
            padding: 20px 20px 20px 20px;
            box-shadow: 0px 0px 3px 0px rgb(125, 125, 125);
            border-radius: 5px;
        }

        .mybtn {
            /* 박스 안의 내용 가운데 정렬 */
            display: flex;
            /* column 세로 정렬 row 가로 정렬 */
            flex-direction: row;
            align-items: center;
            justify-content: center;

            margin-top: 10px;
        }

        .mybtn>button {
            margin-right: 10px;
        }

        label {
            color: rgb(125, 125, 125);
        }

        .modal_btn {
            display: block;
            margin: 40px auto;
            padding: 10px 20px;
            background-color: royalblue;
            border: none;
            border-radius: 5px;
            color: #fff;
            cursor: pointer;
            transition: box-shadow 0.2s;
        }

        .modal_btn:hover {
            box-shadow: 3px 4px 11px 0px #00000040;
        }

        /*모달 팝업 영역 스타일링*/
        .modal {
            /*팝업 배경*/
            display: none;
            /*평소에는 보이지 않도록*/
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            overflow: hidden;
            background: rgba(0, 0, 0, 0.5);
        }

        .modal .modal_popup {
            /*팝업*/
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            padding: 20px;
            background: #ffffff;
            border-radius: 20px;
        }

        .modal .modal_popup .close_btn {
            display: block;
            padding: 10px 20px;
            background-color: rgb(125, 125, 125);
            border: none;
            border-radius: 5px;
            color: #fff;
            cursor: pointer;
            transition: box-shadow 0.2s;
        }

        /* .modal.on {
            display: block;
        } */

        .modal{
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            }

    </style>
    <script type="module">
        // Firebase SDK 라이브러리 가져오기
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-app.js";
        import { getFirestore } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
        import { collection, addDoc } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";
        import { getDocs } from "https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore.js";


        // Firebase 구성 정보 설정
        // For Firebase JS SDK v7.20.0 and later, measurementId is optional
        const firebaseConfig = {
            apiKey: "AIzaSyDElo0jaJLG90QJqG_8dFNZEHbYRJ2xzcY",
            authDomain: "wlcometoyunaworld.firebaseapp.com",
            projectId: "wlcometoyunaworld",
            storageBucket: "wlcometoyunaworld.appspot.com",
            messagingSenderId: "973793714644",
            appId: "1:973793714644:web:2ca1097b5cfb8b0c9c7641",
            measurementId: "G-QNHQVX4CV3"
        };


        // Firebase 인스턴스 초기화
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);


        $("#postingbtn").click(async function () {
            let image = $('#image').val();
            let title = $('#title').val();
            let content = $('#content').val();
            let date = $('#date').val();

            let doc = {
                'image': image,
                'title': title,
                'content': content,
                'date': date
            };
            // 카드를 가지고 오는 addDoc
            await addDoc(collection(db, "albums"), doc);
            alert('저장 완료!');
            window.location.reload();
        })

        $("#savebtn").click(async function () {
            // 디스플레이 논을 시켜주는 toggle()
            $('#postingbox').toggle();
        })


        let url = 'http://openapi.seoul.go.kr:8088/6d4d776b466c656533356a4b4b5872/json/RealtimeCityAir/1/99';
        fetch(url).then(res => res.json()).then(data => {
            let mise = data['RealtimeCityAir']['row'][0]['IDEX_NM'];
            $('#msg').text(mise);
        })

        let image, title, content, date;

        // 카드를 가지고 오는 getDocs
        let docs = await getDocs(collection(db, "albums"));
        docs.forEach((doc) => {
            let row = doc.data();
            // 카드 붙이기, 위에서 받아온 row의 값을 읽어와 화면에 뿌려줘야 한다.
            image = row['image'];
            title = row['title'];
            content = row['content'];
            date = row['date'];

            let temp_html = `
                        <div class="col">
                <div class="card h-100">
                    <img src="${image}"
                        class="card-img-top" alt="...">
                    <div class="card-body">
                        <h5 class="card-title">${title}</h5>
                        <p class="card-text">${content}</p>

                        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
                            더보기
                        </button>

                    </div>
                    <div class="card-footer">
                        <small class="text-muted">${date}</small>
                    </div>
                </div>
            </div>`;
            $('#card').append(temp_html);
        });

        const modal = document.querySelector('.modal');
        const modalOpen = document.querySelector('.modal_btn');
        const modalClose = document.querySelector('.close_btn');

//열기 버튼을 눌렀을 때 모달팝업이 열림
modalOpen.addEventListener('click',function(){
  	//'on' class 추가
    modal.classList.add('on');
}); 
//닫기 버튼을 눌렀을 때 모달팝업이 닫힘
modalClose.addEventListener('click',function(){
    //'on' class 제거
    modal.classList.remove('on');
});
    </script>

</head>

<body>

    <!-- 모달 팝업 -->
    <!-- Scrollable modal -->
    <!-- <div class="modal">
        <div class="modal_popup">
            <h3>모달팝업</h3> 
            <h5 class="card-title">앨범 제목</h5>
            <p class="card-text">앨범 내용</p>
            <small class="text-muted">앨범 날짜</small>
            <button type="button" class="close_btn">닫기</button>
        </div>
    </div>
    --------------------------------------------------------- -->
        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered ">
            <div class="modal-content">
                <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">더보기</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <h5 class="card-title">앨범 제목
                        <div class="card h-100">
                            <img src="${image}"
                            class="card-img-top" alt="picture">
                    </h5>
                    <p class="card-text">앨범 내용</p>
                    <small class="text-muted">앨범 날짜</small>
                </div>
                <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">확인</button>
                <button type="button" class="btn btn-primary" data-bs-dismiss="modal">닫기</button>
                </div>
            </div>
            </div>
        </div>



    <div class="mytitle">
        <h1>나만의 추억앨범</h1>
        <p>현재 서울의 미세먼지 : <span id="msg">좋음</span></p>
        <button id="savebtn">추억 저장하기</button>
    </div>
    <!-- 추억 앨범 올리기 -->
     
    <div class="mypostingbox" id="postingbox">
        <div class="form-floating mb-3">
            <input type="text" class="form-control" id="image" placeholder="앨범 이미지">
            <label for="floatingInput">앨범 이미지</label>
        </div>
        <div class="form-floating mb-3">
            <input type="text" class="form-control" id="title" placeholder="앨범 제목">
            <label for="floatingInput">앨범 제목</label>
        </div>
        <div class="form-floating mb-3">
            <input type="text" class="form-control" id="content" placeholder="앨범 내용">
            <label for="floatingInput">앨범 내용</label>
        </div>
        <div class="form-floating mb-3">
            <input type="text" class="form-control" id="date" placeholder="앨범 날짜">
            <label for="floatingInput">앨범 날짜</label>
        </div>
        <div class="mybtn">
            <button id="postingbtn" type="button" class="btn btn-dark">기록하기</button>
            <button type="button" class="btn btn-outline-dark">닫기</button>
        </div>
    </div>

    <!-- 앨범 리스트 -->
    <div class="mycards">
        <div id="card" class="row row-cols-1 row-cols-md-4 g-4">
            <div class="col">
                <div class="card h-100">
                    <img src="https://images.unsplash.com/photo-1446768500601-ac47e5ec3719?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1446&q=80"
                        class="card-img-top" alt="...">
                    <div class="card-body">
                        <h5 class="card-title">앨범 제목</h5>
                        <!-- <p class="card-text">앨범 내용<button type="button" class="modal_btn">더보기</button></p> -->
                         <!-- Button trigger modal -->
                        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
                            더보기
                        </button>
                    </div>
                    <div class="card-footer">
                        <small class="text-muted">앨범 날짜</small>
                    </div>
                </div>
            </div>
        </div>
    </div>
</body>

</html>
