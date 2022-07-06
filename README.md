<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
          integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
            crossorigin="anonymous"></script>

    <link href="https://fonts.googleapis.com/css2?family=Gowun+Batang:wght@400;700&display=swap" rel="stylesheet">

    <title>선착순 공동구매</title>

    <style>
        * {
            font-family: 'Gowun Batang', serif;
            color: white;
        }

        body {
            background-image: linear-gradient(0deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('https://cdn.aitimes.com/news/photo/202010/132592_129694_3139.jpg');
            background-position: center;
            background-size: cover;
        }

        h1 {
            font-weight: bold;
        }

        .order {
            width: 500px;
            margin: 60px auto 0px auto;
            padding-bottom: 60px;
        }

        .mybtn {
            width: 100%;
        }

        .order > table {
            caption-side: bottom;
            border-collapse: collapse;
        }
        .order > table > thead{
            width: 100%;
        }
        option {
            color: black;
        }
    </style>
    <script>
        $(document).ready(function () {
            show_order();
        });

        function show_order() {
            $.ajax({
                type: 'GET',
                url: '/mars',
                data: {},
                success: function (response) {
                    let rows = response['orders']
                    for(let i = 0; i < rows.length; i++){
                        let name = rows[i]['name']
                        let address = rows[i]['address']
                        let size = rows[i]['size']
                        let price = size.slice(0,2)*500

                        let temp_html = `<tr>
                                            <td>${name}</td>
                                            <td>${address}</td>
                                            <td>${size}</td>
                                            <td>${price}</td>
                                        </tr>`
                        $('#order-box').append(temp_html)
                    }
                }
            });
        }

        function save_order() {
            let name = $('#name').val()
            let address = $('#address').val()
            let size = $('#size').val()

            $.ajax({
                type: 'POST',
                url: '/mars',
                data: {name_give: name, address_give:address, size_give:size},
                success: function (response) {
                    alert(response['msg'])
                    window.location.reload()
                }
            });
        }
    </script>
</head>
<body>
<div class="mask"></div>
<div class="order">
    <h1>화성에 땅 사놓기!</h1>
    <h3>가격: 평 당 500원</h3>
    <p>
        화성에 땅을 사둘 수 있다고?<br/>
        앞으로 백년 간 오지 않을 기회. 화성에서 즐기는 노후!
    </p>
    <div class="order-info">
        <div class="input-group mb-3">
            <span class="input-group-text">이름</span>
            <input id="name" type="text" class="form-control">
        </div>
        <div class="input-group mb-3">
            <span class="input-group-text">주소</span>
            <input id="address" type="text" class="form-control">
        </div>
        <div class="input-group mb-3">
            <label class="input-group-text" for="size">평수</label>
            <select class="form-select" id="size">
                <option selected>-- 주문 평수 --</option>
                <option value="10평">10평</option>
                <option value="20평">20평</option>
                <option value="30평">30평</option>
                <option value="40평">40평</option>
                <option value="50평">50평</option>
            </select>
        </div>
        <button onclick="save_order()" type="button" class="btn btn-warning mybtn">주문하기</button>
    </div>
    <table class="table">
        <thead>
        <tr>
            <th scope="col">이름</th>
            <th scope="col">주소</th>
            <th scope="col">평수</th>
            <th scope="col">가격</th>
        </tr>
        </thead>
        <tbody id="order-box">
        </tbody>
    </table>
</div>
</body>
</html>
