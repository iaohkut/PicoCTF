# IAW302

> Bài này rất đúng với mô tả của người ra đề, một người anh (không hề) lừa: "G(old)". Một ví dụ điển hình cho một lỗi điển hình của PHP String :))

- Truy cập vào bài, ta có login form:

    ![login](img/IAW302_1.png)

- Thử xem source code của bài xem sao? (Source code khá dài, nên mình sẽ cắt từ đoạn form đến hết hint)

    ```html
        <form action="verify.php" method="post">
            User Name:<br>
            <input type="text" name="username"><br><br>
            Password:<br>
            <input type="password" name="password"><br><br>
            <input type="submit" name="submit" value="Login">
        </form>
        <!--
        if(isset($_POST['submit'])){
                if ((int) $_POST['password'] === (int) "8xdeadbeef"){
                    $usr = $_POST['username'];
                    $pas = hash('sha256', htmlentities($_POST['password']));
                    if($pas == "0" && strcmp("ahihi", $usr) == 0 && $usr != "ahihi"){
                        session_start();
                        $_SESSION['logged'] = TRUE;
                        header("Location: chall.php");
                        exit;
                        }
                }else{

                    header("Location: index.php");
                    exit;
                }
        }else{
                header("Location: index.php");
                exit;
        }
        ?>
    ```
