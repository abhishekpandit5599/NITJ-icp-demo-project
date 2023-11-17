# NITJ-icp-demo-project

## BMI CALCULATOR

`main.mo`

```js
actor {
    public func calculateBMI(weightInKg: Float, heightInCM: Float) : async (Float, Text) {
        let bmi = (weightInKg  / heightInCM / heightInCM)*10000;
        let category =
            if (bmi < 18.5) {
                "Underweight"
            } else if (bmi >= 18.5 and bmi < 24.9) {
                "Normal weight"
            } else if (bmi >= 24.9 and bmi < 29.9) {
                "Overweight"
            } else {
                "Obesity"
            };

        return (bmi, category);
    }

};

```

`index.html`

```html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="main.css">
  <title>BMI Calculator</title>
</head>

<body>

  <div class="container">
    <div class="box">
      <h1>BMI Calculator</h1>
      <div class="content">


        <div class="input">
          <label for="height">Age</label>
          <input type="number" class="text-input" id="age" autocomplete="off" required min="1" max="100" />
        </div>

        <div class="gender">

          <label class="container">
            <input type="radio" name="radio" id="m">
            <p class="text">Male</p>
            <span class="checkmark"></span>
          </label>


          <label class="container">
            <input type="radio" name="radio" id="f">
            <p class="text">Female</p>
            <span class="checkmark"></span>
          </label>

        </div>

        <div class="containerHW">
          <div class="inputH">
            <label for="height">Height(cm)</label>
            <input type="number" id="height" required>
          </div>

          <div class="inputW">
            <label for="weight">Weight(kg)</label>
            <input type="number" id="weight" required>
          </div>
        </div>
        <button class="calculate" id="submit">Calculate BMI</button>
      </div>
      <div class="result">
        <p>Your BMI is</p>
        <div id="result">00.00</div>
        <p class="comment"></p>
      </div>
      <div class="footer">
      </div>
    </div>
  </div>
  <!-- The Modal -->
  <div id="myModal" class="modal">
    <!-- Modal content -->
    <div class="modal-content">
      <span class="close">&times;</span>
      <p id="modalText"></p>
    </div>
  </div>
</body>
</html>

```

`index.js`

```js
import { BMI_backend } from "../../declarations/BMI_backend";

var age = document.getElementById("age");
var height = document.getElementById("height");
var weight = document.getElementById("weight");
var male = document.getElementById("m");
var female = document.getElementById("f");
var form = document.getElementById("form");
let resultArea = document.querySelector(".comment");

var modalContent = document.querySelector(".modal-content");
var modalText = document.querySelector("#modalText");
var modal = document.getElementById("myModal");
var span = document.getElementsByClassName("close")[0];

document.getElementById("submit").addEventListener("click", async (e) => {
  e.preventDefault();
  const button = e.target;

  // const name = document.getElementById("name").value.toString();

  button.disabled = true;
  calculate();

  button.disabled = false;

  return false;
});

function calculate() {
  if (
    age.value == "" ||
    height.value == "" ||
    weight.value == "" ||
    (male.checked == false && female.checked == false)
  ) {
    modal.style.display = "block";
    modalText.innerHTML = `All fields are required!`;
  } else {
    countBmi();
  }
}

async function countBmi() {
  var p = [age.value, height.value, weight.value];
  if (male.checked) {
    p.push("male");
  } else if (female.checked) {
    p.push("female");
  }

  const bmi_data = await BMI_backend.calculateBMI(
    parseFloat(p[2]),
    parseFloat(p[1])
  );

  const bmi = bmi_data[0];
  const result = bmi_data[1];

  resultArea.style.display = "block";
  document.querySelector(
    ".comment"
  ).innerHTML = `You are <span id="comment">${result}</span>`;
  document.querySelector("#result").innerHTML = bmi.toFixed(2);
}

// When the user clicks on <span> (x), close the modal
span.onclick = function () {
  modal.style.display = "none";
};

// When the user clicks anywhere outside of the modal, close it
window.onclick = function (event) {
  if (event.target == modal) {
    modal.style.display = "none";
  }
};
```

`main.css`

```css

/* Import Google Font - Poppins */
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap');
    *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Poppins', sans-serif;
    }

    body{
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background: rgb(97,138,254,0.9);
    }
/*
  .container {
    min-width: 250px;
  } */

  .box {
    width: 500px;
    background: #fafafa;
    border-radius: 12px;
    text-align: center;
    box-shadow: 2px 2px 20px 5px rgba(0,0,0,0.5);

  }


  h1 {
    color: rgb(0, 0, 0, 0.7);
    font-weight: bold;
    font-size: 36px;
    padding: 30px 0;

  }
    .content {
        padding: 0 30px;

    }
    .input {
        background: #fff;
        box-shadow: 0px 0px 95px -30px rgba(0, 0, 0, 0.15);
        border-radius: 12px;
        padding: 20px 0;
        margin-bottom: 20px;
    }

    .input label {
        display: block;
        font-size: 18px;
        font-weight: 600;
        color: #000;
        margin-bottom: 20px;
    }
    .input input {
        outline: none;
        border: none;
        border-bottom: 1px solid #4f7df9;
        width: 50%;
        text-align: center;
        font-size: 28px;
        font-family: "Nunito", sans-serif;
    }


    .inputW {
        background: #fff;
        box-shadow: 0px 0px 95px -30px rgba(0, 0, 0, 0.15);
        border-radius: 12px;
        padding: 10px 0;
        margin-bottom: 20px;
    }

    .inputW label {
        display: block;
        font-size: 18px;
        font-weight: 600;
        color: #000;
        margin-bottom: 20px;
    }
   .inputW input {
        outline: none;
        border: none;
        border-bottom: 1px solid #4f7df9;
        width: 50%;
        text-align: center;
        font-size: 28px;
        font-family: "Nunito", sans-serif;
    }

    .inputH {
        background: #fff;
        box-shadow: 0px 0px 95px -30px rgba(0, 0, 0, 0.15);
        border-radius: 12px;
        padding: 10px 0;
        margin-bottom: 20px;
        margin-right: 20px;
    }


    .inputH label {
        display: block;
        font-size: 18px;
        font-weight: 600;
        color: #000;
        margin-bottom: 20px;
    }
   .inputH input {
        outline: none;
        border: none;
        border-bottom: 1px solid #4f7df9;
        width: 50%;
        text-align: center;
        font-size: 28px;
        font-family: "Nunito", sans-serif;
    }


button.calculate {
    cursor: pointer;
    font-family: "Nunito", sans-serif;
    color: #fff;
    background: rgb(97,138,254,1);
    font-size: 16px;
    border-radius: 7px;
    padding: 12px 0;
    width: 100%;
    outline: none;
    border: none;
    transition: all 0.5s;
  }
  button.calculate:hover {
    background: #0044ff;
  }



.result {
    padding: 10px 20px;
  }
  .result p {
    font-weight: 600;
    font-size: 22px;
    color: #000;
    margin-bottom: 15px;
  }
  .result #result {
    font-size: 36px;
    font-weight: 900;
    color: #4f7df9;
    background-color: #eaeaea;
    display: inline-block;
    padding: 7px 20px;
    border-radius: 55px;
    margin-bottom: 25px;
  }
  #comment {
    color: #4f7df9;
    font-weight: 800;
  }

.comment {
    display: none;
    border: dashed 1px;
    border-radius: 7px;
    padding: 5px;
}

.footer {
    display: flex;
    justify-content: start;
    align-items: center;
    padding: 10px 15px
}

.footer-text {
    text-decoration: none;
    color: rgba(40, 40, 40, 0.8);
    font-family: 'Poppins', sans-serif;
    font-weight: 200;
    font-size: 14px;
    transition: all 0.5;
}

.footer-text:hover {
    color: rgba(40, 40, 40, 1);
}

.gender {
    display: flex;
    justify-content: space-around;
    align-items: center;
    align-content: center;
    background: #fff;
    box-shadow: 0px 0px 95px -30px rgba(0, 0, 0, 0.15);
    border-radius: 12px;
    padding: 20px 0;
    margin-bottom: 20px;
}

  /* Style for Radio */
.gender .container {
    display: block;
    position: relative;
    padding-left: 35px;
    /* margin-bottom: 12px; */
    margin-top: 7px;
    cursor: pointer;
    font-size: 22px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  /* Hide the browser's default radio button */
  .gender .container input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
  }

  /* Create a custom radio button */
  .checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 25px;
    width: 25px;
    background-color: #eee;
    border-radius: 50%;
  }

  /* On mouse-over, add a grey background color */
  .gender .container:hover input ~ .checkmark {
    background-color: #ccc;
  }

  /* When the radio button is checked, add a blue background */
  .gender .container input:checked ~ .checkmark {
    background-color: #2196F3;
  }

  /* Create the indicator (the dot/circle - hidden when not checked) */
  .checkmark:after {
    content: "";
    position: absolute;
    display: none;
  }

  /* Show the indicator (dot/circle) when checked */
  .gender .container input:checked ~ .checkmark:after {
    display: block;
  }

  /* Style the indicator (dot/circle) */
  .gender .container .checkmark:after {
       top: 9px;
      left: 9px;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: white;
  }
  /* END Style for Radio */

.containerHW {
    display: flex;
    justify-content: space-around;
    align-items: center;
}

/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 100px; /* Location of the box */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
  padding-top: 300px;

}

/* Modal Content */
.modal-content {
  background-color: #fefefe;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 600px;
  border-radius: 10px;
  box-shadow: #393c76 -1px 2px 2px 1px;
}

#modalText {
  padding-top: 8px;
  padding-right: 5px;
  font-size: 18px;
  font-family: 'Poppins', sans-serif;
  color: rgb(24, 23, 23);
}

.modal-wrong {
border: 2px solid red;
}

.modal-correct {
  border: 2px solid green;
  }

/* The Close Button */
.close {
  color: #aaaaaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.close:hover {
  color: #d41111;
}

.linkDownload {
    position: fixed;
    background-color: #d12322;
    margin: 20px;
    padding: 10px 10px;
    left: -0px;
    border-radius: 5px;
    bottom: -0px;
    font-size: 16px;
    font-weight: 400;
    color: #fff;
    text-decoration: none;
    text-transform: capitalize;
    transition: all 0.43s ease-in-out;
  }

.linkDownload i {
    padding-left: 7px;
  }

  .linkDownload:hover {
    text-decoration: none;
    background-color: black;
  }

@media (max-width: 700px){
    .box {
        width: 380px;
    }

    .input label {

        font-size: 18px;
    }

    .inputH label, .inputW label {
        font-size: 14px;
    }

    .input input, .inputH input, .inputW input{
        font-size: 24px;
    }

    .modal-content {
      width: 380px;
    }
}
```

# TIC-TAC-TOE GAME

`main.mo`

```js
actor {
  type BoxInputData = {
    box0 : Text;
    box1 : Text;
    box2 : Text;
  };

  public query func changeTurn(turn : Text) : async Text {
    var result : Text = "";
    if (turn == "X") {
      result := "0";
    } else {
      result := "X";
    };
    return result;
  };

  public func checkWin(data : BoxInputData) : async Bool {
    if ((data.box0 == data.box1) and (data.box2 == data.box1) and (data.box0 != "")) {
      return true;
    };
    return false;
  };
};
```

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="/main.css">
    <title>Tic Tac Toe</title>
</head>
<body>
    <header>
        <nav id='navbar'>
            <ul>
                <li>TicTacToeGame.com</li>
            </ul>
        </nav>
    </header>

    <div class="gameContainer">
        <div id="container" class="container">
            <div class="box bt-0 bl-0"><span class="boxtext"></span></div>
            <div class="box bt-0"><span class="boxtext"></span></div>
            <div class="box bt-0 br-0"><span class="boxtext"></span></div>
            <div class="box bl-0"><span class="boxtext"></span></div>
            <div class="box"><span class="boxtext"></span></div>
            <div class="box br-0"><span class="boxtext"></span></div>
            <div class="box bb-0 bl-0"><span class="boxtext"></span></div>
            <div class="box bb-0"><span class="boxtext"></span></div>
            <div class="box bb-0 br-0"><span class="boxtext"></span></div>
        </div>

        <div class="gameinfo">
            <h1>Tic Tac Toe Game</h1>
            <div id="status"></div>
            <div>
                <span class="info">Turns of X</span>
                <button id="reset">Reset</button>
            </div>
            <div class="imgbox">
                <img src="excit.gif" alt="">
            </div>
        </div>
    </div>

</body>
</html>

```

`index.js`

```js
import { tic_tac_toe_game_backend } from "../../declarations/tic_tac_toe_game_backend";

let turn = "X";
let gameOver = false;
let gameWorkingNow = false;
let status = document.getElementById("status");

// Function to change the turn
const changeTurn = async () => {
  let nextTurn = await tic_tac_toe_game_backend.changeTurn(turn);
  return nextTurn;
};

// Function to check for a win
const checkWin = async () => {
  gameWorkingNow = true;
  status.innerText = "Processing";
  let boxtext = document.getElementsByClassName("boxtext");
  let wins = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < wins.length; i++) {
    let data = {
      box0: boxtext[wins[i][0]].innerText,
      box1: boxtext[wins[i][1]].innerText,
      box2: boxtext[wins[i][2]].innerText,
    };
    let result = await tic_tac_toe_game_backend.checkWin(data);
    if (result) {
      document.querySelector(".info").innerText =
        "Player " + boxtext[wins[i][0]].innerText + " Won";
      gameOver = true;
      document
        .querySelector(".imgbox")
        .getElementsByTagName("img")[0].style.width = "200px";
      status.innerText = "";
      gameWorkingNow = false;
    }
  }

  status.innerText = "";
  gameWorkingNow = false;
};

// Game Logic
let boxes = document.getElementsByClassName("box");
Array.from(boxes).forEach((element) => {
  let boxtext = element.querySelector(".boxtext");
  element.addEventListener("click", async () => {
    if (boxtext.innerText === "" && !gameOver && !gameWorkingNow) {
      boxtext.innerText = turn;
      turn = await changeTurn();
      await checkWin();
      if (!gameOver) {
        document.getElementsByClassName("info")[0].innerText =
          "Turn for : " + turn;
      }
    }
  });
});

// Reset function
reset.addEventListener("click", (e) => {
  let boxtext = document.querySelectorAll(".boxtext");
  Array.from(boxtext).forEach((element) => {
    element.innerText = "";
  });
  turn = "X";
  document.getElementsByClassName("info")[0].innerText = "Turn for : " + turn;
  gameOver = false;
  document.querySelector(".imgbox").getElementsByTagName("img")[0].style.width =
    "0";
});
```

`main.css`

```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:ital@1&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Baloo+Bhaina+2&display=swap');

*{
    margin: 0px;
    padding: 0px;
}

nav{
    background-color: black;
    color: white;
    height: 45px;
    font-size: 22px;
    display: flex;
    align-items: center;
    font-family: 'Roboto', sans-serif;
}

nav ul{
    list-style-type: none;
}

nav ul li{
    float: left;
    margin: 7px 20px;

}

.gameContainer{
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

.container{
    display: grid;
    grid-template-columns: repeat(3,10vw);
    grid-template-rows: repeat(3,10vw);
    font-family: 'Roboto', sans-serif;
}

.box{
    border: 2px solid black;
    place-items: center;
    font-size: 8vw;
    cursor: pointer;
    display: flex;
    justify-content: center;
}

.container div:hover{
    background-color: rgb(220, 196, 243);
}

.gameinfo{
    margin-left: 20px;
    font-family: 'Baloo Bhaina 2', cursive;
    font-size: 20px
}

.imgbox img{
    width: 0;
    transition: width .1s ease-in-out;
}

.br-0{
    border-right: 0;
}

.bl-0{
    border-left: 0;
}

.bb-0{
    border-bottom: 0;
}

.bt-0{
    border-top: 0;
}

.gameinfo span{
    color: rgb(240, 80, 6);
    border: 3px solid black;
    border-radius: 9px;
    padding: 5px 20px;
    margin-left: 40px;
}

.gameinfo button{
    padding: 13px 20px;
    border-radius: 9px;
    background-color: black;
    color: whitesmoke;
}

.gameinfo h1{
    margin: 20px 5px;
    padding: 50px ;
    font-size: 3vw;
}

@media screen and (max-width: 900px) {
    .gameContainer{
        flex-wrap: wrap;
    }
    .gameinfo h1{
        font-size: 1.5rem;
        @import url('https://fonts.googleapis.com/css2?family=Roboto:ital@1&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Baloo+Bhaina+2&display=swap');

*{
    margin: 0px;
    padding: 0px;
}

nav{
    background-color: black;
    color: white;
    height: 45px;
    font-size: 22px;
    display: flex;
    align-items: center;
    font-family: 'Roboto', sans-serif;
}

nav ul{
    list-style-type: none;
}

nav ul li{
    float: left;
    margin: 7px 20px;

}

.gameContainer{
    display: flex;
    justify-content: center;
    margin-top: 20px;
}

.container{
    display: grid;
    grid-template-columns: repeat(3,10vw);
    grid-template-rows: repeat(3,10vw);
    font-family: 'Roboto', sans-serif;
}

.box{
    border: 2px solid black;
    place-items: center;
    font-size: 8vw;
    cursor: pointer;
    display: flex;
    justify-content: center;
}

.container div:hover{
    background-color: rgb(220, 196, 243);
}

.gameinfo{
    margin-left: 20px;
    font-family: 'Baloo Bhaina 2', cursive;
    font-size: 20px
}

.imgbox img{
    width: 0;
    transition: width .1s ease-in-out;
}

.br-0{
    border-right: 0;
}

.bl-0{
    border-left: 0;
}

.bb-0{
    border-bottom: 0;
}

.bt-0{
    border-top: 0;
}

.gameinfo span{
    color: rgb(240, 80, 6);
    border: 3px solid black;
    border-radius: 9px;
    padding: 5px 20px;
    margin-left: 40px;
}

.gameinfo button{
    padding: 13px 20px;
    border-radius: 9px;
    background-color: black;
    color: whitesmoke;
}

.gameinfo h1{
    margin: 20px 5px;
    padding: 50px ;
    font-size: 3vw;
}

@media screen and (max-width: 900px) {
    .gameContainer{
        flex-wrap: wrap;
    }
    .gameinfo h1{
        font-size: 1.5rem;
        margin-top: 0;
    }
    .container{
        grid-template-columns: repeat(3,20vw);
        grid-template-rows: repeat(3,20vw);
    }
}margin-top: 0;
    }
    .container{
        grid-template-columns: repeat(3,20vw);
        grid-template-rows: repeat(3,20vw);
    }
}
```

## AGE-CALCULATOR

`main.mo`

```js
actor {
  type TimeData = {
    day: Nat32;
    month: Nat32;
    year: Nat32;
  };

  type TimeDataInput = {
    date: Nat32;
    month: Nat32;
    year: Nat32;
  };

  public func ageCalculate(currentData : TimeDataInput, birthData: TimeDataInput) : async TimeData {
    var years = currentData.year - birthData.year;
    var months : Nat32 = 0;
    var days : Nat32 = 0;

    if(currentData.month >= birthData.month){
      months := currentData.month - birthData.month;
    }else{
      months := birthData.month - currentData.month;
      years -= 1;
      months := 12 - months;
    };

    if(currentData.date >= birthData.date){
      days := currentData.date - birthData.date;
    }else{
      days := birthData.date - currentData.date;
      days := days + (30 - currentData.date);
      months -= 1;
    };

    var data = {
      day = days;
      month = months;
      year = years;
    };
    return data;
  };
};

```

`main.css`

```css
body {
    font-family: sans-serif;
    font-size: 1.1rem;
}

img {
    max-width: 50vw;
    max-height: 25vw;
    display: block;
    margin: auto;
}

form {
    display: flex;
    justify-content: center;
    gap: 0.5em;
    flex-flow: row wrap;
    max-width: 40vw;
    margin: auto;
    align-items: baseline;
}

button[type="submit"] {
    padding: 5px 20px;
    margin: 10px auto;
    float: right;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f5f5f5;
    margin: 0;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

.calculator {
    box-sizing: border-box;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
    padding: 30px;
    text-align: center;
    max-width: 400px;
    width: 100%;
}

h1 {
    color: #333;
}

.input-container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
}

.input-container label {
    margin-bottom: 10px;
    font-weight: bold;
}

.input-container input {
    width: 50%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    outline: none;
}

#calculate-button {
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    padding: 10px 20px;
    cursor: pointer;
    font-weight: bold;
    margin-top: 20px;
}

#result {
    margin-top: 20px;
    word-wrap: break-word;
}
```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Age Calculator</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  </head>
  <body>
    <main>
      <div class="calculator">
        <h2>Age Calculator</h2>
        <div class="input-container">
            <label for="birthdate">Enter your birthdate:</label>
            <input type="date" id="birthdate">
        </div>
        <button id="calculate-button">Calculate Age</button>
        <div id="result"></div>
    </div>
    </main>
  </body>
</html>

```

`index.js`

```js
import { age_calculator_backend } from "../../declarations/age_calculator_backend";

const calculateButton = document.getElementById("calculate-button");
const result = document.getElementById("result");

calculateButton.addEventListener("click", async () => {
  try {
    result.textContent = "";
    const birthTime = new Date(document.getElementById("birthdate").value);
    let currentTime = new Date();

    let currentYear = currentTime.getFullYear();
    let birthYear = birthTime.getFullYear();
    let currentMonth = currentTime.getMonth();
    let birthMonth = birthTime.getMonth();
    let currentDate = currentTime.getDate();
    let birthDate = birthTime.getDate();

    let currentData = {
      date: currentDate,
      month: currentMonth,
      year: currentYear,
    };

    let birthData = {
      date: birthDate,
      month: birthMonth,
      year: birthYear,
    };

    let data = await age_calculator_backend.ageCalculate(
      currentData,
      birthData
    );

    result.textContent = `${data.year} years ${data.month} months ${data.day} days`;
  } catch (error) {
    result.textContent = error;
  }
});
```


## Ball Game


`main.mo`

```js
actor {
  public func enterName(name : Text) : async Text {
    let namee : Text = name;
    return "Hello, " # namee # "! You r a good player....";
  };

};

```

`main.css`

```css

* {
  margin: 0;
  padding: 0;
}

body {
  background: #f2f2f2;
}

canvas {
  display:block;
  margin: 40px auto 20px;
  border: 1px solid #333;
  box-shadow: 0 0 16px 2px rgba(0,0,0,0.8);
}

p, a {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 19px;
  color: #777;
  display: block;
  width: 400px;
  margin: 0 auto;
  text-align: center;
  text-decoration:none;
}

#playername {
  font-size: 24px; /* Adjust the font size as needed */
  font-weight: bold; /* You can make the text bold if you like */
  text-align: center; /* Center the text within the element */
  color: #3498db; /* Change the color to your preference */
  background-color: #f2f2f2; /* Add a background color if desired */
  padding: 10px; /* Add padding to create some spacing around the text */
  border-radius: 5px; /* Add rounded corners to the element */
}

.info {
  margin:14px auto;
  text-align: justify;
  font-size: 18px;
  color: #999;
}

a {
  color:#3377ee;
}
```
`index.html`

```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width,user-scalable=no">
    <meta name="viewport" content="width=device-width" />
    <title>buildBlocks</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  </head>
  <body>
    <p id="score"></p>
    <p id="playername"></p>
      <canvas id="canvas"></canvas>
  </body>
</html>
```

`index.js`

```js

import { buildBlocks_backend } from "../../declarations/buildBlocks_backend";

var a = prompt("Enter your name!");

var b = document.getElementById('playername');
b.textContent = await buildBlocks_backend.enterName(a);

var map = {

  tile_size: 16,

  keys: [
    { id: 0, colour: '#333', solid: 0 },
    { id: 1, colour: '#888', solid: 0 },
    { id: 2, colour: '#555', solid: 1, bounce: 0.35 },
    { id: 3, colour: 'rgba(121, 220, 242, 0.4)', friction: { x: 0.9, y: 0.9 }, gravity: { x: 0, y: 0.1 }, jump: 1, fore: 1 },
    { id: 4, colour: '#777', jump: 1 },
    { id: 5, colour: '#E373FA', solid: 1, bounce: 1.1 },
    { id: 6, colour: '#666', solid: 1, bounce: 0 },
    { id: 7, colour: '#73C6FA', solid: 0, script: 'change_colour' },
    { id: 8, colour: '#FADF73', solid: 0, script: 'next_level' },
    { id: 9, colour: '#C93232', solid: 0, script: 'death' },
    { id: 10, colour: '#555', solid: 1 },
    { id: 11, colour: '#0FF', solid: 0, script: 'unlock' }
  ],

  data: [
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 6, 6, 6, 6, 6, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 2, 1, 1, 1, 1, 1, 2, 2, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 7, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 2, 2, 2, 4, 2, 2, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2, 2, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 4, 2, 1, 2],
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2],
    [2, 1, 2, 2, 1, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2],
    [2, 1, 2, 2, 2, 1, 1, 1, 1, 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2],
    [2, 1, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 4, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 8, 1, 1, 1, 2],
    [2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 6, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2],
    [2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 9, 9, 9, 2, 10, 10, 10, 10, 10, 10, 1, 1, 1, 1, 1, 1, 1, 11, 2, 2, 2, 2, 4, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 10, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 6, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 2, 1, 1, 1, 1, 1, 1, 2],
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 4, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 1, 2],
    [2, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 6, 6, 6, 2, 2, 2, 2, 2, 2, 6, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2],
    [2, 1, 1, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 1, 1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 1, 1, 1, 1, 2, 5, 5, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 1, 1, 1, 1, 1, 1, 1, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2],
    [2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 5, 5, 5, 1, 1, 1, 1, 1, 2, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 2],
    [2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2]
  ],

  gravity: {
    x: 0,
    y: 0.3
  },

  vel_limit: {
    x: 2,
    y: 16
  },

  movement_speed: {
    jump: 6,
    left: 0.3,
    right: 0.3
  },


  player: {
    x: 2,
    y: 2,
    colour: '#FF9900'
  },

  scripts: {
    change_colour: 'game.player.colour = "#"+(Math.random()*0xFFFFFF<<0).toString(16);',
    next_level: 'alert("Yay! You won! Reloading map.");game.load_map(map);',
    death: 'alert("You died!");game.load_map(map);',
    unlock: 'game.current_map.keys[10].solid = 0;game.current_map.keys[10].colour = "#888";'
  }
};

var Clarity = function () {

  this.alert_errors = false;
  this.log_info = true;
  this.tile_size = 16;
  this.limit_viewport = false;
  this.jump_switch = 0;

  this.viewport = {
    x: 200,
    y: 200
  };

  this.camera = {
    x: 0,
    y: 0
  };

  this.key = {
    left: false,
    right: false,
    up: false
  };

  this.player = {

    loc: {
      x: 0,
      y: 0
    },

    vel: {
      x: 0,
      y: 0
    },

    can_jump: true
  };

  window.onkeydown = this.keydown.bind(this);
  window.onkeyup = this.keyup.bind(this);
};

Clarity.prototype.error = function (message) {

  if (this.alert_errors) alert(message);
  if (this.log_info) console.log(message);
};

Clarity.prototype.log = function (message) {

  if (this.log_info) console.log(message);
};

Clarity.prototype.set_viewport = function (x, y) {

  this.viewport.x = x;
  this.viewport.y = y;
};

Clarity.prototype.keydown = function (e) {

  var _this = this;

  switch (e.keyCode) {
    case 37:
      _this.key.left = true;
      break;
    case 38:
      _this.key.up = true;
      break;
    case 39:
      _this.key.right = true;
      break;
  }
};

Clarity.prototype.keyup = function (e) {

  var _this = this;

  switch (e.keyCode) {
    case 37:
      _this.key.left = false;
      break;
    case 38:
      _this.key.up = false;
      break;
    case 39:
      _this.key.right = false;
      break;
  }
};

Clarity.prototype.load_map = function (map) {

  if (typeof map === 'undefined'
    || typeof map.data === 'undefined'
    || typeof map.keys === 'undefined') {

    this.error('Error: Invalid map data!');

    return false;
  }

  this.current_map = map;

  this.current_map.background = map.background || '#333';
  this.current_map.gravity = map.gravity || { x: 0, y: 0.3 };
  this.tile_size = map.tile_size || 16;

  var _this = this;

  this.current_map.width = 0;
  this.current_map.height = 0;

  map.keys.forEach(function (key) {

    map.data.forEach(function (row, y) {

      _this.current_map.height = Math.max(_this.current_map.height, y);

      row.forEach(function (tile, x) {

        _this.current_map.width = Math.max(_this.current_map.width, x);

        if (tile == key.id)
          _this.current_map.data[y][x] = key;
      });
    });
  });

  this.current_map.width_p = this.current_map.width * this.tile_size;
  this.current_map.height_p = this.current_map.height * this.tile_size;

  this.player.loc.x = map.player.x * this.tile_size || 0;
  this.player.loc.y = map.player.y * this.tile_size || 0;
  this.player.colour = map.player.colour || '#000';

  this.key.left = false;
  this.key.up = false;
  this.key.right = false;

  this.camera = {
    x: 0,
    y: 0
  };

  this.player.vel = {
    x: 0,
    y: 0
  };

  this.log('Successfully loaded map data.');

  return true;
};

Clarity.prototype.get_tile = function (x, y) {

  return (this.current_map.data[y] && this.current_map.data[y][x]) ? this.current_map.data[y][x] : 0;
};

Clarity.prototype.draw_tile = function (x, y, tile, context) {

  if (!tile || !tile.colour) return;

  context.fillStyle = tile.colour;
  context.fillRect(
    x,
    y,
    this.tile_size,
    this.tile_size
  );
};

Clarity.prototype.draw_map = function (context, fore) {

  for (var y = 0; y < this.current_map.data.length; y++) {

    for (var x = 0; x < this.current_map.data[y].length; x++) {

      if ((!fore && !this.current_map.data[y][x].fore) || (fore && this.current_map.data[y][x].fore)) {

        var t_x = (x * this.tile_size) - this.camera.x;
        var t_y = (y * this.tile_size) - this.camera.y;

        if (t_x < -this.tile_size
          || t_y < -this.tile_size
          || t_x > this.viewport.x
          || t_y > this.viewport.y) continue;

        this.draw_tile(
          t_x,
          t_y,
          this.current_map.data[y][x],
          context
        );
      }
    }
  }

  if (!fore) this.draw_map(context, true);
};

Clarity.prototype.move_player = function () {

  var tX = this.player.loc.x + this.player.vel.x;
  var tY = this.player.loc.y + this.player.vel.y;

  var offset = Math.round((this.tile_size / 2) - 1);

  var tile = this.get_tile(
    Math.round(this.player.loc.x / this.tile_size),
    Math.round(this.player.loc.y / this.tile_size)
  );

  if (tile.gravity) {

    this.player.vel.x += tile.gravity.x;
    this.player.vel.y += tile.gravity.y;

  } else {

    this.player.vel.x += this.current_map.gravity.x;
    this.player.vel.y += this.current_map.gravity.y;
  }

  if (tile.friction) {

    this.player.vel.x *= tile.friction.x;
    this.player.vel.y *= tile.friction.y;
  }

  var t_y_up = Math.floor(tY / this.tile_size);
  var t_y_down = Math.ceil(tY / this.tile_size);
  var y_near1 = Math.round((this.player.loc.y - offset) / this.tile_size);
  var y_near2 = Math.round((this.player.loc.y + offset) / this.tile_size);

  var t_x_left = Math.floor(tX / this.tile_size);
  var t_x_right = Math.ceil(tX / this.tile_size);
  var x_near1 = Math.round((this.player.loc.x - offset) / this.tile_size);
  var x_near2 = Math.round((this.player.loc.x + offset) / this.tile_size);

  var top1 = this.get_tile(x_near1, t_y_up);
  var top2 = this.get_tile(x_near2, t_y_up);
  var bottom1 = this.get_tile(x_near1, t_y_down);
  var bottom2 = this.get_tile(x_near2, t_y_down);
  var left1 = this.get_tile(t_x_left, y_near1);
  var left2 = this.get_tile(t_x_left, y_near2);
  var right1 = this.get_tile(t_x_right, y_near1);
  var right2 = this.get_tile(t_x_right, y_near2);


  if (tile.jump && this.jump_switch > 15) {

    this.player.can_jump = true;

    this.jump_switch = 0;

  } else this.jump_switch++;

  this.player.vel.x = Math.min(Math.max(this.player.vel.x, -this.current_map.vel_limit.x), this.current_map.vel_limit.x);
  this.player.vel.y = Math.min(Math.max(this.player.vel.y, -this.current_map.vel_limit.y), this.current_map.vel_limit.y);

  this.player.loc.x += this.player.vel.x;
  this.player.loc.y += this.player.vel.y;

  this.player.vel.x *= .9;

  if (left1.solid || left2.solid || right1.solid || right2.solid) {

    /* fix overlap */

    while (this.get_tile(Math.floor(this.player.loc.x / this.tile_size), y_near1).solid
      || this.get_tile(Math.floor(this.player.loc.x / this.tile_size), y_near2).solid)
      this.player.loc.x += 0.1;

    while (this.get_tile(Math.ceil(this.player.loc.x / this.tile_size), y_near1).solid
      || this.get_tile(Math.ceil(this.player.loc.x / this.tile_size), y_near2).solid)
      this.player.loc.x -= 0.1;

    /* tile bounce */

    var bounce = 0;

    if (left1.solid && left1.bounce > bounce) bounce = left1.bounce;
    if (left2.solid && left2.bounce > bounce) bounce = left2.bounce;
    if (right1.solid && right1.bounce > bounce) bounce = right1.bounce;
    if (right2.solid && right2.bounce > bounce) bounce = right2.bounce;

    this.player.vel.x *= -bounce || 0;

  }

  if (top1.solid || top2.solid || bottom1.solid || bottom2.solid) {

    /* fix overlap */

    while (this.get_tile(x_near1, Math.floor(this.player.loc.y / this.tile_size)).solid
      || this.get_tile(x_near2, Math.floor(this.player.loc.y / this.tile_size)).solid)
      this.player.loc.y += 0.1;

    while (this.get_tile(x_near1, Math.ceil(this.player.loc.y / this.tile_size)).solid
      || this.get_tile(x_near2, Math.ceil(this.player.loc.y / this.tile_size)).solid)
      this.player.loc.y -= 0.1;

    /* tile bounce */

    var bounce = 0;

    if (top1.solid && top1.bounce > bounce) bounce = top1.bounce;
    if (top2.solid && top2.bounce > bounce) bounce = top2.bounce;
    if (bottom1.solid && bottom1.bounce > bounce) bounce = bottom1.bounce;
    if (bottom2.solid && bottom2.bounce > bounce) bounce = bottom2.bounce;

    this.player.vel.y *= -bounce || 0;

    if ((bottom1.solid || bottom2.solid) && !tile.jump) {

      this.player.on_floor = true;
      this.player.can_jump = true;
    }

  }

  // adjust camera

  var c_x = Math.round(this.player.loc.x - this.viewport.x / 2);
  var c_y = Math.round(this.player.loc.y - this.viewport.y / 2);
  var x_dif = Math.abs(c_x - this.camera.x);
  var y_dif = Math.abs(c_y - this.camera.y);

  if (x_dif > 5) {

    var mag = Math.round(Math.max(1, x_dif * 0.1));

    if (c_x != this.camera.x) {

      this.camera.x += c_x > this.camera.x ? mag : -mag;

      if (this.limit_viewport) {

        this.camera.x =
          Math.min(
            this.current_map.width_p - this.viewport.x + this.tile_size,
            this.camera.x
          );

        this.camera.x =
          Math.max(
            0,
            this.camera.x
          );
      }
    }
  }

  if (y_dif > 5) {

    var mag = Math.round(Math.max(1, y_dif * 0.1));

    if (c_y != this.camera.y) {

      this.camera.y += c_y > this.camera.y ? mag : -mag;

      if (this.limit_viewport) {

        this.camera.y =
          Math.min(
            this.current_map.height_p - this.viewport.y + this.tile_size,
            this.camera.y
          );

        this.camera.y =
          Math.max(
            0,
            this.camera.y
          );
      }
    }
  }

  if (this.last_tile != tile.id && tile.script) {

    eval(this.current_map.scripts[tile.script]);
  }

  this.last_tile = tile.id;
};

Clarity.prototype.update_player = function () {

  if (this.key.left) {

    if (this.player.vel.x > -this.current_map.vel_limit.x)
      this.player.vel.x -= this.current_map.movement_speed.left;
  }

  if (this.key.up) {

    if (this.player.can_jump && this.player.vel.y > -this.current_map.vel_limit.y) {

      this.player.vel.y -= this.current_map.movement_speed.jump;
      this.player.can_jump = false;
    }
  }

  if (this.key.right) {

    if (this.player.vel.x < this.current_map.vel_limit.x)
      this.player.vel.x += this.current_map.movement_speed.left;
  }

  this.move_player();
};

Clarity.prototype.draw_player = function (context) {

  context.fillStyle = this.player.colour;

  context.beginPath();

  context.arc(
    this.player.loc.x + this.tile_size / 2 - this.camera.x,
    this.player.loc.y + this.tile_size / 2 - this.camera.y,
    this.tile_size / 2 - 1,
    0,
    Math.PI * 2
  );

  context.fill();
};

Clarity.prototype.update = function () {

  this.update_player();
};

Clarity.prototype.draw = function (context) {

  this.draw_map(context, false);
  this.draw_player(context);
};

/* Setup of the engine */

window.requestAnimFrame =
  window.requestAnimationFrame ||
  window.webkitRequestAnimationFrame ||
  window.mozRequestAnimationFrame ||
  window.oRequestAnimationFrame ||
  window.msRequestAnimationFrame ||
  function (callback) {
    return window.setTimeout(callback, 1000 / 60);
  };

var canvas = document.getElementById('canvas'),
  ctx = canvas.getContext('2d');

canvas.width = 400;
canvas.height = 400;

var game = new Clarity();
game.set_viewport(canvas.width, canvas.height);
game.load_map(map);

/* Limit the viewport to the confines of the map */
game.limit_viewport = true;

var Loop = function () {

  ctx.fillStyle = '#333';
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  game.update();
  game.draw(ctx);

  window.requestAnimFrame(Loop);
};

Loop();

```



## Certification system in (REACT)


`main.mo`

```js

import HashMap "mo:base/HashMap";
import Principal "mo:base/Principal";
import List "mo:base/List";
import Iter "mo:base/Iter";
import Time "mo:base/Time";
import Debug "mo:base/Debug";

actor certification {

  private stable var registeredEntries : [(Principal, User)] = [];

  // private stable var savedList : [Principal] = [];

  type HashMap<K, V> = HashMap.HashMap<K, V>;

  stable var registedOnesList : List.List<Principal> = List.nil<Principal>();

  var registedUsersHashmap : HashMap<Principal, User> = HashMap.HashMap<Principal, User>(5, Principal.equal, Principal.hash);

  let certificateHolders : HashMap<Principal, Certification> = HashMap.HashMap<Principal, Certification>(5, Principal.equal, Principal.hash);

  // Registeration
  type User = {
    name : Text;
    isRegistered : Bool;
  };

  type Certification = {
    name : Text;
    issueTimestamp : Int;
    isRevoked : Bool;
  };

  public shared (msg) func registerUser(namee : Text) : async Text {

    let obj : User = { name = namee; isRegistered = true };

    let userId : Principal = msg.caller;

    var iter = Iter.fromList(registedOnesList);

    for (user in iter) {
      if (user == userId) {
        return "you are already registered buddy!";
      };
    };

    registedOnesList := Iter.toList(iter);

    registedUsersHashmap.put(userId, obj);
    registedOnesList := List.push(userId, registedOnesList);

    return "success registering!!";

  };

  public func getCertificate(address : Principal) : async Text {

    var iter = Iter.fromList(registedOnesList);
    var userFound : Bool = false;

    let certiName : Text = "Blockchain Panjab Certificate";

    for (user in iter) {
      if (user == address) {

        let obj : Certification = {
          name = certiName;
          issueTimestamp = Time.now();
          isRevoked = false;
        };

        certificateHolders.put(address, obj);

        Debug.print(certiName # "Certificate has been issued!");
        userFound := true;
        // return "you are already registered buddy!";
      } 
    };

    if(userFound == false){
      return "user has not registered himself";
    };

    registedOnesList := Iter.toList(iter);

    return "Certificate has been issued!";

  };

  public shared(msg) func getId(): async Principal{
    return msg.caller;
  };

  public query func getUsers() : async List.List<Principal> {
    return registedOnesList;
  };

  system func preupgrade() {
    registeredEntries := Iter.toArray(registedUsersHashmap.entries());
  };

  system func postupgrade() {

    registedUsersHashmap := HashMap.fromIter<Principal, User>(registeredEntries.vals(), 1, Principal.equal, Principal.hash);


      
    // if (balances.size() < 1) {
    //   balances.put(owner, totalSupply);
    // };
  };

};
```

`main.css`

```css
body {
    font-family: sans-serif;
    font-size: 1.5rem;
}

img {
    max-width: 50vw;
    max-height: 25vw;
    display: block;
    margin: auto;
}

form {
    display: flex;
    justify-content: center;
    gap: 0.5em;
    flex-flow: row wrap;
    max-width: 40vw;
    margin: auto;
    align-items: baseline;
}

button[type="submit"] {
    padding: 5px 20px;
    margin: 10px auto;
    float: right;
}

#greeting {
    margin: 10px auto;
    padding: 10px 60px;
    border: 1px solid #222;
}

#greeting:empty {
    display: none;
}
```

`index.jsx`

```js

import React from "react";
import ReactDOM from "react-dom";
import App from "./components/App";

ReactDOM.render(<App />, document.getElementById("root"));
```

`App.jsx`

```js

import React from 'react';
import CertificateApp from './CertificateApp';


function App() {
  return (
    <div className="App">
      <CertificateApp/>
    </div>
  );
}

export default App;
```

`CertificateApp.jsx`

```js

import React, { useState } from 'react';
import './CertificateApp.css'; // Import your CSS file
import TextDisplay from './TextDisplay'; // Make sure to provide the correct path
import { certiSys_backend } from '../../../declarations/certiSys_backend';

function CertificateApp() {
  const [name, setName] = useState('');
  const [inputData, setInputData] = useState('');
  const [registeredMsg, setRegisteredMsg] = useState('');
  const [id, setId] = useState('');

  const handleNameChange = async (e) => {
    setName(e.target.value);
  };

  const handleInputDataChange = (e) => {
    setInputData(e.target.value);
  };

  const handleRegister = async () => {
    const regMsg = await certiSys_backend.registerUser(name);
    setRegisteredMsg(regMsg);
  };

  const getIdd = async () => {
    const id = await certiSys_backend.getId();
    setId(id);
  }

  return (
    <div className="container">
      <h1>Register Yourself for a Certification</h1>
      <div>
        <label>
          Enter Your Name:
          <input type="text" value={name} onChange={handleNameChange} />
        </label>
        <button onClick={handleRegister}>Register</button>
      </div>
      {registeredMsg && <div className="result">{registeredMsg}</div>}
      <div className="id-section">
        <h1>Get Your ID</h1>
        {id ? "your id is "+ id : (
          <button className="id-button" onClick={getIdd}>Get ID</button>
        )}
      </div>
      <TextDisplay className="text-display" />
    </div>
  );
}

export default CertificateApp;
```
`TextDisplay.jsx`

```js

import React, { useState } from 'react';
import { certiSys_backend } from '../../../declarations/certiSys_backend';
import {Principal} from '@dfinity/principal'

function TextDisplay() {
  const [inputId, setInputId] = useState('');
  const [displayText, setDisplayText] = useState('');

  const handleIdChange = async (e) => {
    setInputId(e.target.value);
   
  };

  const handleDisplayText = async () => {
    console.log(inputId)

    const message = await certiSys_backend.getCertificate(Principal.fromText(inputId));
    console.log(message);
    setDisplayText(message);
    console.log(displayText);
   
  };

  return (
    <div>
      <h1>Get your certificate !! </h1>
      <div>
        <label>
          Enter your ID :
          <input type="text" value={inputId} onChange={handleIdChange} />
        </label>
        <button onClick={handleDisplayText}>Get Certificate!</button>
      </div>
      {displayText && (
        <div>
          <p>Text Displayed: {displayText} </p>
        </div>
      )}
    </div>
  );
}

export default TextDisplay;

```

`package.json`

```js
// inside package.json update depedencies

"dependencies": {
    "@dfinity/agent": "^0.19.3",
    "@dfinity/candid": "^0.19.3",
    "@dfinity/principal": "^0.19.3",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "@material-ui/core": "^4.12.4",
    "@material-ui/icons": "^4.11.3",
    "@mui/icons-material": "^5.14.14",
    "@mui/material": "^5.14.14",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.31",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "ts-loader": "^9.5.0"
  }

```

`tsconfig.json`

```js
//add this where webpack.config.js is present 

{
    "compilerOptions": {
      "target": "es2018",        /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
      "lib": ["ES2018", "DOM"],  /* Specify library files to be included in the compilation. */
      "allowJs": true,           /* Allow javascript files to be compiled. */
      "jsx": "react",            /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    },
    "include": ["src/**/*"]
  }
  
```

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Document</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  	<link id="external-css" rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Montserrat&amp;display=swap" media="all">
</head>
  <body>
    <div id="root"></div>
  </body>
</html>

```



## Mobile-Calcy in (REACT)


`main.mo`

```js
import Float "mo:base/Float";
actor Calculator {

  var num : Float = 0;

  public func add(val1 : Float, val2 : Float) : async Float {
    num := val1 +val2;
    return num;
  };

  public func subtract(val1 : Float, val2 : Float) : async Float {
    num := val1 -val2;
    return num;
  };

  public func multiply(val1 : Float, val2 : Float) : async Float {
    num := val1 * val2;
    return num;
  };

  public func divison(val1 : Float, val2 : Float) : async ?Float {
    if (val2 == 0) {
      return null;
    } else {
      num := val1 / val2;
      return ?num;
    };
  };

  public func clearAll() : async Float {
    num := 0;
    return num;
  };

};
```

`main.css`

```css
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
}

.App {
  width: 390px;
  margin: 100px auto;
  align-items: center;
}

.screen {
  width: 300px;
  height: 80px;
  text-align: right;
  border: 3px solid rgb(88, 87, 87);
  font-size: 30px;
  padding: 10px;
  border-radius: 10px;
  background-color: white;
}

.button {
  width: 80px;
  height: 80px;
  background-color: black;
  color: white;
  border-radius: 10px;
  font-size: 25px;
}

body {
  background-color: lightgray;
}

.btn {
  width: 160px;
  background-color: white;
  color: black;
}

.opr {
  background-color: rgb(158, 55, 7);
}

.btn2 {
  background-color: rgb(65, 63, 63);
}

.button1 {
  width: 160px;
  background-color: yellowgreen;
}
```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>calcy</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  	<link id="external-css" rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Montserrat&amp;display=swap" media="all">
</head>
  <body>
    <div id="root"></div>
  </body>
</html>

```

`index.jsx`

```js
import React from "react";
import ReactDOM from "react-dom";
import App from "./Components/App";

ReactDOM.render(<App />, document.getElementById("root"))

```

`App.jsx`

```js
import React, { useState } from "react";
import { calcy_backend } from "../../../declarations/calcy_backend";

function App() {
  const [curr, setCurr] = useState("");
  const [prev, setPrev] = useState("");
  const [operand, setOperand] = useState("");

  const numHandler = (e) => {
    if (curr.includes(".") && e.target.value === ".") return;
    curr ? setCurr((prev) => prev + e.target.value) : setCurr(e.target.value);
  };

  const clearHandler = async () => {
    await calcy_backend.clearAll();
    setCurr("");
    setPrev("");
    setOperand("");
  };
  const clearLastHandler = () => {
    if (operand) return;
    let res = curr.slice(0, -1);
    setCurr(res);
  };

  const operandHandler = (e) => {
    setOperand(e.target.value);
    if (curr === "") return;
    if (prev !== "") calculate();
    else {
      setPrev(curr);
      setCurr("");
    }
  };
  const calculate = async () => {
    let cal, val1, val2;
    switch (operand) {
      case "+":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.add(val1, val2));
        break;

      case "-":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.subtract(val1, val2));
        break;

      case "*":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.multiply(val1, val2));
        break;

      case "/":
        val1 = Number(prev);
        val2 = Number(curr);
        cal = String(await calcy_backend.divison(val1, val2));
        break;
      default:
        return;
    }
    console.log("cal", cal);

    setPrev(cal);
    setCurr("");
  };

  let numArray = [];
  for (let i = 0; i <= 9; i++) {
    numArray.push(i);
  }
  numArray.push(".");

  const operandsss = ["+", "-", "*", "/"];

  return (
    <div className="App">
      <h1>calculator</h1>
      <div className="screen">
        <span>{prev}</span>
        <span>{operand}</span>
        <span>{curr}</span>
      </div>
      {operandsss.map((opr, index) => (
        <button
          key={index}
          value={opr}
          className="button opr"
          onClick={operandHandler}
        >
          {opr}
        </button>
      ))}
      {numArray.map((num, index) => (
        <button key={index} value={num} className="button" onClick={numHandler}>
          {num}
        </button>
      ))}
      <input
        type="button"
        value="X"
        className="button btn2"
        onClick={clearLastHandler}
      />
      <input
        type="button"
        value="AC"
        className="button btn"
        onClick={clearHandler}
      />
      <input
        type="button"
        value="="
        className="button button1"
        onClick={calculate}
      />
    </div>
  );
}

export default App;
```

`package.json`

```js
// change your depedencies with this in package.json

"dependencies": {
    "@dfinity/agent": "^0.19.3",
    "@dfinity/candid": "^0.19.3",
    "@dfinity/principal": "^0.19.3",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "@material-ui/core": "^4.12.4",
    "@material-ui/icons": "^4.11.3",
    "@mui/icons-material": "^5.14.14",
    "@mui/material": "^5.14.14",
    "autoprefixer": "^10.4.16",
    "postcss": "^8.4.31",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "ts-loader": "^9.5.0"
  },
```

`tsconfig.json`

```js
//add this where webpack.config.js is present 

{
    "compilerOptions": {
      "target": "es2018",        /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */
      "lib": ["ES2018", "DOM"],  /* Specify library files to be included in the compilation. */
      "allowJs": true,           /* Allow javascript files to be compiled. */
      "jsx": "react",            /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    },
    "include": ["src/**/*"]
  }

```

## todo list without login


`main.mo`

```js
import Text "mo:base/Text";
import List "mo:base/List";
import Debug "mo:base/Debug";
import Nat "mo:base/Nat";

actor {

  public type Note = {
    textarea : Text;
    id : Nat;
  };

  var notes : List.List<Note> = List.nil<Note>();

  public func addTodo(title : Text) : async Nat {
    let todoId = List.size(notes) +1;

    let newNote : Note = {
      id = todoId;
      textarea = title;
    };

    notes := List.push(newNote, notes);
    Debug.print(debug_show (notes));

    return newNote.id;
  };

  public query func getAllTodo() : async [Note] {
    return List.toArray(notes);

  };

  public func deleteTodo(id : Nat) {
    Debug.print(debug_show (id));

    notes := List.filter(
      notes,
      func(note : Note) : Bool {
        return note.id != id;
      },
    );

    Debug.print("id is deleted");
  };

};

```

`main.css`

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Times New Roman", Times, serif;
}

body {
  background-image: linear-gradient(to right, #605e7e, #6b73c1, #37b3cc);
  margin: 50px 2%;
}

.container-title {
  font-size: 25px;
  text-align: center;
  margin-bottom: 30px;
  color: white;
  text-shadow: 3px 1px black;
}

.inputcol {
  display: grid;
  column-gap: 5px;
  grid-template-columns: 60% 10%;
  justify-content: center;
  margin-top: 10px;
  margin-bottom: 20px;
}

.textarea {
  min-height: 50px;
  max-width: 100%;
  border-radius: 10px;
  border-color: #333;
  font-size: 20px;
  padding: 10px;
  overflow: auto;
  overflow-x: hidden;
}

.textarea::-webkit-scrollbar-track {
  border-radius: 10px;
  background-color: #333;
}

.textarea::-webkit-scrollbar {
  width: 10px;
  cursor: pointer;
}

.textarea::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #8f8acc;
}

.textarea:focus {
  outline: none;
}

.buttoninput {
  border-radius: 10px;
  border-color: #333;
  background-color: white;
  font-size: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.buttoninput:hover {
  background-color: #8f8acc;
  color: white;
}

#todolist .item {
  background-color: #fff;
  border-radius: 8px;
  margin-bottom: 10px;
  padding: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: space-between;
  word-wrap: break-word;
  word-break: break-all;
  font-size: 20px;
}

.trash-button {
  background-color: transparent;
  color: black;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.trash-button:hover {
  background-color: #6b73c1;
  color: white;
  transform: scale(1.05);
}

.trash-button:active {
  transform: scale(0.95);
}

.fa-check,
.fa-trash {
  pointer-events: none;
}

```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="main.css" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css"
    />
    <title>ToDo List</title>
  </head>
  <body>
    <div class="container-title">
      <h1>TO DO LIST</h1>
    </div>
    <div class="container">
      <form class="inputcol">
        <textarea
          class="textarea"
          type="text"
          name="textarea"
          placeholder="Add todo"
          required
        ></textarea>
        <button class="buttoninput" type="submit">
          <i class="fa-solid fa-check"></i>
        </button>
      </form>
    </div>
    <ul id="todolist"></ul>
    <script src="index.js"></script>
  </body>
</html>

```

`index.js`

```js
import { todo_backend } from "../../declarations/todo_backend";

const textarea = document.querySelector(".textarea");
const appointForm = document.querySelector(".inputcol");

appointForm.addEventListener("submit", async (e) => {
  e.preventDefault();
  const obj = {
    textarea: textarea.value,
  };

  try {
    const getId = await todo_backend.addTodo(obj.textarea);
    showUserOnScreen({ id: getId, textarea: obj.textarea });
    textarea.value = ""; 
  } catch (error) {
    console.error("Error adding todo:", error);
  }
});

window.addEventListener("DOMContentLoaded", async () => {
  try {
    const getAll = await todo_backend.getAllTodo();
    getAll.forEach((todo) => showUserOnScreen(todo));
  } catch (error) {
    console.error("Error fetching todo list:", error);
  }
});

function showUserOnScreen(user) {
  const parentNode = document.getElementById("todolist");

  const newListItem = document.createElement("li");
  newListItem.className = "item";
  newListItem.id = Number(user.id);
  newListItem.textContent = user.textarea;

  const deleteButton = document.createElement("button");
  deleteButton.className = "trash-button";
  deleteButton.innerHTML = '<i class="fa-solid fa-trash"></i>';
  deleteButton.onclick = () => deleteTodo(user.id);

  newListItem.appendChild(deleteButton);
  parentNode.appendChild(newListItem);
}

async function deleteTodo(userId) {
  try {
    console.log("userId in del =>", userId);
    await todo_backend.deleteTodo(userId);
    removeUserFromScreen(userId);
  } catch (error) {
    console.error("Error deleting todo:", error);
  }
}

function removeUserFromScreen(userId) {
  const childNodeToBeDeleted = document.getElementById(userId);
  if (childNodeToBeDeleted) {
    childNodeToBeDeleted.parentNode.removeChild(childNodeToBeDeleted);
  }
}

```


## SNAKE GAME

`main.mo`

```js
actor {
   stable var highestScore : Nat = 0;

  public func updateHighestScore(score : Nat) {
    if (score > highestScore) {
      highestScore := score;
    };
  };

  public query func highestScoreHandler() : async Nat {
    return highestScore;
  }
};

```

`main.css`

```css
body {
  text-align: center;
  font-family: "Arial", sans-serif;
  background-color: #f5f5f5;
  margin: 0;
  padding: 0;
}

h1 {
  color: #007bff;
  font-size: 36px;
  font-weight: bold;
  margin-top: 20px;
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

canvas {
  border: 2px solid #555;
  background-color: #eee;
  margin: 20px auto;
  display: block;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.alert {
  padding: 10px;
  margin: 10px;
  color: #fff;
  font-weight: bold;
  border-radius: 5px;
  display: inline-block;
}

.alert-success {
  background-color: #28a745;
}

.alert-danger {
  background-color: #dc3545;
}

.button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 15px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 20px;
}

.button:hover {
  background-color: #0056b3;
}

```
`index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <title>towerBuilding</title>
    <base href="/" />
    <link rel="icon" href="favicon.ico" />
    <link type="text/css" rel="stylesheet" href="main.css" />
  </head>
  <body>
    <h1>Snake Game 🪱</h1>
    <canvas id="stage" height="400" width="520"></canvas>
  </body>
</html>

```

`index.js`

```js
import { towerBuilding_backend } from "../../declarations/towerBuilding_backend";

var Game = Game || {};
var Keyboard = Keyboard || {};
var Component = Component || {};

/**
 * Keyboard Map
 */
Keyboard.Keymap = {
  37: "left",
  38: "up",
  39: "right",
  40: "down",
};

/**
 * Keyboard Events
 */
Keyboard.ControllerEvents = function () {
  var self = this;
  this.pressKey = null;
  this.keymap = Keyboard.Keymap;

  document.onkeydown = function (event) {
    self.pressKey = event.which;
  };

  this.getKey = function () {
    return this.keymap[this.pressKey];
  };
};

/**
 * Game Component Stage
 */
Component.Stage = function (canvas, conf, callback) {
  this.keyEvent = new Keyboard.ControllerEvents();
  this.width = canvas.width;
  this.height = canvas.height;
  this.length = [];
  this.food = {};
  this.direction = "right";
  this.isGameOver = false;
  this.conf = {
    cw: 10,
    size: 5,
    fps: 1000,
  };

  if (typeof conf === "object") {
    for (var key in conf) {
      if (conf.hasOwnProperty(key)) {
        this.conf[key] = conf[key];
      }
    }
  }

  // Fetch the highest score from the backend
  towerBuilding_backend
    .highestScoreHandler()
    .then((highestScore) => {
      this.highestScore = highestScore;
      this.score = 0;
      if (typeof callback === "function") {
        callback(this);
      }
    })
    .catch((error) => {
      console.error("Error fetching highest score:", error);
      this.highestScore = 0;
    });
};

/**
 * Game Component Snake
 */
Component.Snake = function (canvas, conf, callback) {
  new Component.Stage(canvas, conf, (stage) => {
    this.stage = stage;

    this.initSnake = function () {
      this.stage.length = [];
      for (var i = 0; i < this.stage.conf.size; i++) {
        this.stage.length.push({ x: i, y: 0 });
      }
    };

    this.initSnake();

    this.initFood = function () {
      this.stage.food = {
        x: Math.round(
          (Math.random() * (this.stage.width - this.stage.conf.cw)) /
            this.stage.conf.cw
        ),
        y: Math.round(
          (Math.random() * (this.stage.height - this.stage.conf.cw)) /
            this.stage.conf.cw
        ),
      };
    };

    this.initFood();

    this.restart = async function () {
      this.stage.length = [];
      this.stage.food = {};
      this.stage.score = 0;
      this.stage.direction = "right";
      this.stage.keyEvent.pressKey = null;
      this.stage.isGameOver = false;
      this.initSnake();
      this.initFood();
    };

    if (typeof callback === "function") {
      callback(this);
    }
  });
};

/**
 * Game Draw
 */
Game.Draw = function (context, snake) {
  this.drawStage = async function () {
    if (snake.stage.length.length === 0) {
      console.error("Snake length array is empty.");
      return;
    }

    var keyPress = snake.stage.keyEvent.getKey();
    if (typeof keyPress != "undefined") {
      snake.stage.direction = keyPress;
    }

    context.fillStyle = "white";
    context.fillRect(0, 0, snake.stage.width, snake.stage.height);

    var nx = snake.stage.length[0].x;
    var ny = snake.stage.length[0].y;

    switch (snake.stage.direction) {
      case "right":
        nx++;
        break;
      case "left":
        nx--;
        break;
      case "up":
        ny--;
        break;
      case "down":
        ny++;
        break;
    }

    if (!snake.stage.isGameOver && this.collision(nx, ny)) {
      snake.stage.isGameOver = true;
      await towerBuilding_backend.updateHighestScore(snake.stage.score);

      // Fetch and update the highest score after the game ends
      try {
        const newHighestScore =
          await towerBuilding_backend.highestScoreHandler();
        snake.stage.highestScore = newHighestScore;
      } catch (error) {
        console.error("Error fetching updated highest score:", error);
      }

      const restartGame = confirm(
        "Game over! Your score: " +
          snake.stage.score +
          ". Do you want to restart?"
      );
      if (restartGame) {
        await snake.restart();
      } else {
      }
      return;
    }

    if (nx === snake.stage.food.x && ny === snake.stage.food.y) {
      var tail = { x: nx, y: ny };
      snake.stage.score++;
      snake.initFood();
    } else {
      var tail = snake.stage.length.pop();
      tail.x = nx;
      tail.y = ny;
    }
    snake.stage.length.unshift(tail);

    for (var i = 0; i < snake.stage.length.length; i++) {
      var cell = snake.stage.length[i];
      this.drawCell(cell.x, cell.y);
    }

    this.drawCell(snake.stage.food.x, snake.stage.food.y);
    context.fillText("Highest Score: " + snake.stage.highestScore, 5, 20);
    context.fillText(
      "Current Score: " + snake.stage.score,
      5,
      snake.stage.height - 5
    );
  };

  this.drawCell = function (x, y) {
    context.fillStyle = "rgb(170, 170, 170)";
    context.beginPath();
    context.arc(
      x * snake.stage.conf.cw + 6,
      y * snake.stage.conf.cw + 6,
      4,
      0,
      2 * Math.PI,
      false
    );
    context.fill();
  };

  this.collision = function (nx, ny) {
    if (
      nx == -1 ||
      nx == snake.stage.width / snake.stage.conf.cw ||
      ny == -1 ||
      ny == snake.stage.height / snake.stage.conf.cw
    ) {
      return true;
    }
    return false;
  };
};

/**
 * Game Snake
 */
Game.Snake = function (elementId, conf) {
  var canvas = document.getElementById(elementId);
  var context = canvas.getContext("2d");

  new Component.Snake(canvas, conf, (snake) => {
    var gameDraw = new Game.Draw(context, snake);
    setInterval(function () {
      gameDraw.drawStage();
    }, snake.stage.conf.fps);
  });
};

/**
 * Window Load
 */
window.onload = function () {
  const startGame = confirm("Do you want to start the game?");
  if (startGame) {
    new Game.Snake("stage", { fps: 120, size: 4 });
    window.addEventListener("beforeunload", function (event) {
      alert("Game over! Thanks for playing!");
    });
  } else {
    alert("Game not started. Have a great day!");
  }
};

```


## Rgb Game


`main.mo`

```js
import Int "mo:base/Int";


actor {
  var attempts : Int = 0;
  var rightGuess : Int = 0;
  var wrongGuess : Int = 0;

  public func attemptsRightWrong(a : Int, r : Int, w : Int) {
    attempts := a;
    rightGuess := r;
    wrongGuess := w;
  };

  public query func getAttempts() : async Int{
    return attempts;
  };

public query func getRightGuess() : async Int{
    return rightGuess;
  };

  public query func getWrongGuess() : async Int{
    return wrongGuess;
  }

};
```

`main.css`

```css
body {
	background-color: #232323;
	font-family: "Raleway", Sans-serif;
	font-size: 62.5%; /*1 em = 10px */
	margin: 0;
}

#container {
	max-width: 600px;
	margin: 0 auto;
	padding-top: 20px;

}
.square {
	width: 30%;
	background-color: purple;
	padding-bottom: 30%;
	float: left;
	margin: 1.66%;
	transition: background .5s;
	-webkit-transition: background .5s;
	-moz-transition: background .5s;
	o-transition: background .5s;
} 

h1 {
	color: white;
	background-color: #2C8E99;
	font-size: 3.5em;
	text-align: center;
	text-transform: uppercase;
	margin: 0;
	padding: 10px 0;
}

#color-display {
	display: block;
	font-size: 2em;
}

#stripe {
	background: white;
	height: 30px;
	text-align: center;
	color: black;
}

#message {
	text-transform: uppercase;
	color: #2C8E99;
	font-size: 1.5em;
	display: inline-block;
	width: 20%;
}

button {
	outline: none;
	color: #2C8E99;
	font-family: "Raleway", Sans-serif;
	text-transform: uppercase;
	font-size: 1.5em;
	background-color: white;
	height: 100%;
	letter-spacing: 1px;
	transition: all 0.3s;
	-webkit-transition: all 0.3s;
	-moz-transition: all 0.3s;
	o-transition: all 0.3s;
}

button:hover {
	background-color: #2C8E99;
	color: white;
}

.selected {
	background-color: #2C8E99;
	color: white;
}

.square {
	border-radius: 20%;
}

* {
	border: 0px solid red;
}
```
`index.html`

```html
<html>
<head>
	<title>Color Game</title>
	<link rel="stylesheet" type="text/css" href="main.css">
	<link href='https://fonts.googleapis.com/css?family=Raleway:300' rel='stylesheet' type='text/css'>	
<style>
	 #score {
            font-size: 24px;
            font-weight: bold;
            /* color: #4CAF50;  */
			color: white;
			/* Green color */
            margin-top: 420px;
			margin-left : 550px;
        }
</style>
	

</head>
<body>
	<h1>The Great <span id="color-display">RGB</span> Guessing Game</h1>
	<div id ="stripe">
		<button id="reset">New Colors</button>
		<span id="message"></span>
		<button class="mode">Easy</button>
		<button class="mode selected">Hard</button>
	</div>
	<div id="container">
		<div class="square"></div>
		<div class="square"></div>
		<div class="square"></div>
		<div class="square"></div>
		<div class="square"></div>
		<div class="square"></div>
	</div>
  <div id="score"></div>

	<!-- <script type="text/javascript" src="script.js"></script> -->
<!-- 	<script type="text/javascript" src="colorGame.js"></script>
 --></body>
</html>
```

`index.js`

```js
import { rgb_backend } from '../../declarations/rgb_backend';


var numSquares = 6;
var colors = [];
var pickedColor;
var attempts = 0;
var rightGuess = 0;
var wrongGuess = 0;



var squares = document.querySelectorAll(".square");
var colorDisplay = document.querySelector("#color-display");
var messageDisplay = document.querySelector("#message");
var h1 = document.querySelector("h1");
var resetButton = document.querySelector("#reset");
var modeButtons = document.querySelectorAll(".mode");
var easyButton = document.querySelector(".mode");

init();

function init() {
	colorDisplay.textContent = pickedColor;
	setupSquares();
	setupMode();
	reset();
}

resetButton.addEventListener("click", function () {
	reset();
});

const score = document.getElementById("score");


function setupSquares() {
	for (var i = 0; i < squares.length; i++) {
		squares[i].style.backgroundColor = colors[i];
		squares[i].addEventListener("click", function () {
			var clickedColor = this.style.backgroundColor;
			if (clickedColor === pickedColor) {
				messageDisplay.textContent = "Correct";
				resetButton.textContent = "Play Again";
				changeColors(pickedColor);
				attempts = attempts + 1;
				rightGuess++
				rgb_backend.attemptsRightWrong(attempts, rightGuess, wrongGuess);
				score.textContent = "Attempts: "+ attempts+ " Right Guess: "+ rightGuess + "   Wrong Guess: " + wrongGuess;
				console.log(rightGuess, wrongGuess, attempts, 'r', 'w', 'a');
			}
			else {
				this.style.backgroundColor = "#232323";
				messageDisplay.textContent = "try again";
				attempts++;
				wrongGuess++;
				rgb_backend.attemptsRightWrong(attempts, rightGuess, wrongGuess);
				score.textContent = "Attempts: "+ attempts+ " Right Guess: "+ rightGuess + "   Wrong Guess: " + wrongGuess;
				console.log(rightGuess, wrongGuess, attempts, 'r', 'w', 'a');
			}
		});
	}
}




function setupMode() {
	for (var i = 0; i < modeButtons.length; i++) {
		modeButtons[i].addEventListener("click", function () {
			for (var i = 0; i < modeButtons.length; i++) {
				modeButtons[i].classList.remove("selected");
			}
			this.classList.add("selected");
			if (this.textContent === "Easy") {
				numSquares = 3;
			}
			else {
				numSquares = 6;
			}
			reset();
		});
	}
}

function reset() {
	colors = genRandomColors(numSquares);
	pickedColor = chooseColor();
	colorDisplay.textContent = pickedColor;
	h1.style.backgroundColor = "#2C8E99";
	resetButton.textContent = "New Colors";
	messageDisplay.textContent = "";
	for (var i = 0; i < squares.length; i++) {
		if (colors[i]) {
			squares[i].style.display = "block";
			squares[i].style.backgroundColor = colors[i];
		}
		else {
			squares[i].style.display = "none";
		}
	}
}

function changeColors(color) {
	for (var i = 0; i < squares.length; i++) {
		squares[i].style.backgroundColor = color;
		h1.style.backgroundColor = color;
	}
}

function chooseColor() {
	var random = Math.floor(Math.random() * colors.length);
	return colors[random];
}

function genRandomColors(num) {
	var arr = [];
	for (var i = 0; i < num; i++) {
		arr.push(makeColor());
	}
	return arr;
}

function makeColor() {
	var r = Math.floor(Math.random() * 256);
	var g = Math.floor(Math.random() * 256);
	var b = Math.floor(Math.random() * 256);
	return "rgb(" + r + ", " + g + ", " + b + ")";
}




console.log("right", rightGuess);
console.log("wrong", wrongGuess);
console.log("attempts", attempts);


console.log(rightGuess, wrongGuess, attempts, 'r', 'w', 'a');

```


