# Calculator App

The Calculator App is a Flutter-based mobile application designed to provide a simple and intuitive interface for performing basic calculations. By leveraging Flutter’s cross-platform capabilities, this app ensures a consistent and seamless user experience across both iOS and Android devices.

## Features

- **Basic Operations:** Supports addition, subtraction, multiplication, and division.
- **User-friendly Interface:** A clean and modern design that enhances usability.
- **Memory Functions:** Includes memory features for storing and recalling values.
- **Clear Button:** Quickly reset the calculator to start fresh.

## Tech Stack

- **Flutter:** The primary framework used for developing the app, providing a rich set of features for mobile app development.
- **Dart:** The programming language used for developing the app, known for its efficiency and ease of use with Flutter.

## Dependencies

The app utilizes several dependencies to deliver its functionalities:

- `cupertino_icons`: For iOS-style icons.
- `math_expressions`: For evaluating mathematical expressions.

## Getting Started

To run this project locally, ensure you have Flutter installed. Follow these steps:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/calculator-app.git
   ```
2. **Navigate to the project directory:**
   ```bash
   cd calculator-app
   ```
3. **Install dependencies:**
   ```bash
   flutter pub get
   ```
4. **Run the app:**
   ```bash
   flutter run
   ```

   import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'dart:io';
import 'package:flutter_signature_pad/flutter_signature_pad.dart';
import 'package:math_expressions/math_expressions.dart';

void main() {
  runApp(CalculatorApp());
}

class CalculatorApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Calculator',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: CalculatorHome(),
    );
  }
}

class CalculatorHome extends StatefulWidget {
  @override
  _CalculatorHomeState createState() => _CalculatorHomeState();
}

class _CalculatorHomeState extends State<CalculatorHome> {
  String display = '';
  File? _image;
  final _signatureKey = GlobalKey<SignatureState>();

  void buttonPressed(String value) {
    setState(() {
      display += value;
    });
  }

  void calculateResult() {
    try {
      final expression = Parser().parse(display);
      final evaluator = const ExpressionEvaluator();
      final result = evaluator.eval(expression, null);
      setState(() {
        display = result.toString();
      });
    } catch (e) {
      setState(() {
        display = 'Error';
      });
    }
  }

  void clearDisplay() {
    setState(() {
      display = '';
    });
  }

  Future<void> pickImage() async {
    final pickedFile = await ImagePicker().pickImage(source: ImageSource.camera);
    if (pickedFile != null) {
      setState(() {
        _image = File(pickedFile.path);
      });
    }
  }

  void clearSignature() {
    _signatureKey.currentState?.clear();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Calculator'),
        actions: [
          IconButton(
            icon: Icon(Icons.camera),
            onPressed: pickImage,
          ),
          IconButton(
            icon: Icon(Icons.create),
            onPressed: () {
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: Text('Handwriting Input'),
                  content: Column(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      Signature(
                        key: _signatureKey,
                        height: 300,
                        width: 300,
                        backgroundColor: Colors.grey[300]!,
                      ),
                      SizedBox(height: 10),
                      ElevatedButton(
                        onPressed: () {
                          clearSignature();
                          Navigator.of(context).pop();
                        },
                        child: Text('Clear'),
                      ),
                    ],
                  ),
                ),
              );
            },
          ),
        ],
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: <Widget>[
          Container(
            padding: EdgeInsets.all(24),
            alignment: Alignment.centerRight,
            child: Text(
              display,
              style: TextStyle(fontSize: 48, fontWeight: FontWeight.bold),
            ),
          ),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              _buildButton('1'),
              _buildButton('2'),
              _buildButton('3'),
              _buildButton('+'),
            ],
          ),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              _buildButton('4'),
              _buildButton('5'),
              _buildButton('6'),
              _buildButton('-'),
            ],
          ),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              _buildButton('7'),
              _buildButton('8'),
              _buildButton('9'),
              _buildButton('*'),
            ],
          ),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              _buildButton('0'),
              _buildButton('C', isClear: true),
              _buildButton('=', isEqual: true),
              _buildButton('/'),
            ],
          ),
          if (_image != null)
            Padding(
              padding: const EdgeInsets.all(16.0),
              child: Image.file(_image!),
            ),
        ],
      ),
    );
  }

  Widget _buildButton(String value, {bool isClear = false, bool isEqual = false}) {
    return ElevatedButton(
      onPressed: () {
        if (isClear) {
          clearDisplay();
        } else if (isEqual) {
          calculateResult();
        } else {
          buttonPressed(value);
        }
      },
      child: Text(value, style: TextStyle(fontSize: 24)),
    );
  }
}



## Output

Below is a screenshot of the Calculator app's output:




![Screenshot 2024-10-12 225810](https://github.com/user-attachments/assets/6ac05d38-fd4a-49eb-8b8c-1adeaae8970c)

