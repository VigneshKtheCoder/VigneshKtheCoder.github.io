<!DOCTYPE html>
<html lang="en">
    <head>
        <link rel="stylesheet" href="style.css">
    </head>
    <h1 style="font-size: 800%;">
        Altaria     
    </h1>
    <title>
        Altaria: The Best App
    </title>
    <body style="background-color: rgb(48, 140, 119);">
        <div class="wrapper">
            <form action="">
                <h1 style="font-size: 400%;">
                    <u></u><b>Login</b></u>
                </h1>
                <div class="input-box" style="min-height: 5vh;">
                    <input type="text" placeholder="Username or Email" required style="text-align: center;">
                </div>
                <div class="input-box">
                    <input type="password" placeholder="Password" required style="text-align: center;">
                </div>
            </form>
            <div>   
                <div class="remember-forgot">

                    <label> 
                        <input type="checkbox"> Remember me
                    </label>
                    <div>
                        <a href="#"> 
                            Forgot password?
                        </a>
                    </div>
                    <button type="submit" class="btn">
                        Login! 
                    </button>
                    </div>
                    <div class="Register">
                        <p style="min-height: 4vh;">
                            Don't have an Account? <a href="new web 1"> Register </a>
                        </p>
                    </div>
                </div>
                <div>
                    <p style="min-height: 5vh;"> --------or-------- </p>
                    <div class="Google" style="min-height: 5vh;">
                        <button onclick="document.location=''">Sign in with Google</button>
                    </div>
                    <div class="Apple">
                        <button>Sign in with Apple</button>
                    </div>
                </div>
        </div>
    </body>
</html>





*{
    margin: 0%;
    padding: 0%;
    box-sizing: border-box;
}


body{   
    display: grid;
    justify-content: center;
    align-items: center;
    text-align: center;
    min-height: 100vh;
    background-position: center;  
}

.wrapper{
    width: 420px;
    background: transparent;
    border: 2px solid rgba(255, 255, 255, .2);
    padding: 30px 40px;
    background-color: rgb(68, 250, 113);
}

