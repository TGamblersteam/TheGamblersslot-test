<!DOCTYPE html>  <html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <title>tGt Slot Machine</title>  
    <style>  
        body {   
            font-family: Arial, sans-serif;   
            text-align: center;   
            background: linear-gradient(to bottom, #FFD700, #FFA500);   
            color: black;   
        }  
        .container {  
            max-width: 600px;  
            margin: auto;  
            padding: 20px;  
            background: #333;  
            border-radius: 15px;  
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.8);  
            color: white;  
        }  
        h1 {  
            color: #FFD700;  
            text-shadow: 2px 2px 10px rgba(255, 255, 0, 0.8);  
        }  
        .slot-machine {  
            display: flex;  
            justify-content: center;  
            margin: 20px 0;  
            background: #222;  
            padding: 10px;  
            border-radius: 10px;  
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.5);  
        }  
        .reel {  
            width: 80px;  
            height: 80px;  
            border: 3px solid gold;  
            margin: 5px;  
            display: flex;  
            align-items: center;  
            justify-content: center;  
            font-size: 40px;  
            background-color: black;  
            color: white;  
            transition: transform 1s ease-out;  
            border-radius: 10px;  
        }  
        .buttons {  
            margin-top: 20px;  
        }  
        button {  
            padding: 12px 30px;  
            font-size: 18px;  
            margin: 5px;  
            cursor: pointer;  
            border: none;  
            border-radius: 10px;  
            font-weight: bold;  
        }  
        #spin {  
            background: gold;  
            color: black;  
            box-shadow: 0 0 10px rgba(255, 215, 0, 0.8);  
        }  
        .message {  
            margin-top: 15px;  
            font-size: 20px;  
            font-weight: bold;  
            color: white;  
            text-shadow: 2px 2px 8px gold;  
        }  
        .status {  
            font-size: 18px;  
            margin-top: 15px;  
            font-weight: bold;  
        }  
    </style>  
</head>  
<body>
