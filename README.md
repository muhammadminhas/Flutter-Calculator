# Calculator App Using Flutter & Dart.

### For this App Material Design is Implemented.
## GUI Design:
A class named Calculator is made which extends StatelessWidgets so it never changes the state,Also this class is the starting point of our App/Program it Defines the theme and Home of the App where all of the logic is implemented.
```dart
class Calculator extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title:'Calculator',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: SimpleCalculator(),
    );

  }
}
```
The other Class is Simple Calculator which extends StatefullWidgets(So it can change Dynamically) in this class all of the GUI elements and Widgets are Defined.

We are going to use Scaffold widget to implement the basic material design visual layout structure.

First of all the app bar is designed and in app bar title "Calculator" title is Given.

```dart
return Scaffold(
      appBar: AppBar(
        centerTitle: true,
        title: Text('Calculator'),
      ),
      
```
Equation Field and The result field is designed inside Container Widget (helps in desigining multiple fields and manage them efficiently through width, height, padding,font, background color)

Equation Field:

```dart
Container(
            alignment: Alignment.centerRight,
            padding: EdgeInsets.fromLTRB(10, 20, 10, 0),
            child: Text(equation,style: TextStyle(fontSize: equationFontSize),),
          ),
    
```
Result Field:
```dart
 Container(
            alignment: Alignment.centerRight,
            padding: EdgeInsets.fromLTRB(10, 20, 10, 0),
            child: Text(result,style: TextStyle(fontSize: resultFontSize),),
          ),

```
Calculator Numbers and Operators are build in Row Widget(A widget that displays its children in a horizontal array)
As there are more than One row of Numbers and Operators Table is used more specifically TableRow Child is used.
All of these Elements are Wrapped in a Container Widget.

These Elements will cover .75 Width size
```dart
width: MediaQuery.of(context).size.width *.75,
```
Numbers and Operators Design:
```dart
    Container(
            width: MediaQuery.of(context).size.width *.75,
                child: Table(
                  children: [
                    TableRow(
                      children: [
                      buildButton("C", 1, Colors.blueGrey.shade900),
                        buildButton("⌫", 1, Colors.blue),
                        buildButton("÷", 1, Colors.blue),
                      ],
                    ),
                    TableRow(
                      children: [
                        buildButton("7", 1, Colors.black54),
                        buildButton("8", 1, Colors.black54),
                        buildButton("9", 1, Colors.black54),
                      ],
                    ),
                    TableRow(
                      children: [
                        buildButton("4", 1, Colors.black54),
                        buildButton("5", 1, Colors.black54),
                        buildButton("6", 1, Colors.black54),
                      ],
                    ),
                    TableRow(
                      children: [
                        buildButton("1", 1, Colors.black54),
                        buildButton("2", 1, Colors.black54),
                        buildButton("3", 1, Colors.black54),
                      ],
                    ),
                    TableRow(
                      children: [
                        buildButton(".", 1, Colors.black54),
                        buildButton("0", 1, Colors.black54),
                        buildButton("00", 1, Colors.black54),
                      ],
                    ),
                  ],
                ),
              ),
```

Remaining Operators are *,+,-,=. These elements will be placed on Right side and will Cover About remaining .25 Width.

```dart
 width:MediaQuery.of(context).size.width*0.25,
 ```

```dart
Container(
                width:MediaQuery.of(context).size.width*0.25,
                child:Table(
                  children: [
                    TableRow(
                      children: [
                        buildButton("×", 1, Colors.blue),

                      ]
                    ),
                    TableRow(
                        children: [
                          buildButton("-", 1, Colors.blue),

                        ]
                    ),
                    TableRow(
                        children: [
                          buildButton("+", 1, Colors.blue),

                        ]
                    ),
                    TableRow(
                        children: [
                          buildButton("=", 2, Colors.blueGrey.shade900),

                        ]
                    ),
                  ]
                )
              )

```

For desiging each button seperate a function named as "buildButton" which has parameters of ButtonText,height and color.
```dart
Widget buildButton(String buttonText,double buttonHeight,Color buttonColor)
```
In this function buttons are designed also in Container Widget.
Height of each button is taken from the parameter which sets according to whole app screen context.
```dart
height: MediaQuery.of(context).size.height*0.1*buttonHeight,
```
Colors are also taken from parameters which can be seen in implementation when Numbers And Operators where Placed in Tables.

FlatButton widget(FlatButton is the material design widget in flutter. It is a text label material widget that performs an action when the button is tapped) is used for the desiging of buttons which sets the color,shape,width and style of the button and also text Font Size and Color.

```dart
 return Container(
        height: MediaQuery.of(context).size.height*0.1*buttonHeight,
        color: buttonColor,
        child:FlatButton(
            shape:RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(0.0),
              side: BorderSide(
                color: Colors.white,
                width: 1,
                style: BorderStyle.solid,
              ),
            ),
            padding: EdgeInsets.all(16.0),
            onPressed:()=>buttonPressed(buttonText),
            child:Text(
              buttonText,
              style: TextStyle(
                  fontSize: 30.0,
                  fontWeight:FontWeight.normal,
                  color: Colors.white
              ),
            )
        )
    );
```
### This covers all the GUI part of the App

#

## Logic Implementation

For all the calculatons and reponse regarding the buttons pressed a function named as buttonPressed is used which is linked with onpressed functionality of FlatButton Widget.
```dart
onPressed:()=>buttonPressed(buttonText),
```
This function initially creates equation and result variable which is displayed on the top.

It then setStates for each buttons.
```dart
buttonPressed(String buttonText){
    setState(() {
      if(buttonText=="C"){
          equation="0";
          result="0";
          equationFontSize=38.0;
          resultFontSize=48.0;
      }else if(buttonText=="⌫"){
        equationFontSize=48.0;
        resultFontSize=38.0;
         equation=equation.substring(0,equation.length-1);
         if(equation=="")
           {
             equation="0";
           }
      }else if(buttonText=="="){
        equationFontSize=38.0;
        resultFontSize=48.0;
        expression=equation;
        expression=expression.replaceAll('×', '*');
        expression=expression.replaceAll('÷', '/');
        try{
          Parser p=Parser();
          Expression exp=p.parse(expression);
          ContextModel cm=ContextModel();
          result='${exp.evaluate(EvaluationType.REAL, cm)}';
        }catch(e){
          result="Error";
      }

      }else{
        equationFontSize=48.0;
        resultFontSize=38.0;
        if(equation=="0")
          {
            equation=buttonText;
          }else{
        equation=equation+buttonText;
      }
      }
    });
```
For the Calculation a package of math_expression is used.

To install the package this command was executed:
```dart
 $ flutter pub add math_expressions
 ```
 All the calculations is done when "=" sign is pressed and the logic is also implemented in that button conditional statement.

Try and Catch method is used to check if any error occurs if does then it displays the message of "ERROR".

```dart
else if(buttonText=="="){
        equationFontSize=38.0;
        resultFontSize=48.0;
        expression=equation;
        expression=expression.replaceAll('×', '*');
        expression=expression.replaceAll('÷', '/');
        try{
          Parser p=Parser();
          Expression exp=p.parse(expression);
          ContextModel cm=ContextModel();
          result='${exp.evaluate(EvaluationType.REAL, cm)}';
        }catch(e){
          result="Error";
      }

```

Expression is evaluated for signs and it does all the calculations and the final result is stored in result variable.
