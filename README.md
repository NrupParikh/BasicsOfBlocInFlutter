# Basics Of Bloc In Flutter
Flutter Bloc Pattern : Sink and Stream

## Why Bloc Pattern

Issue
----

- Build method called multiple time
- Widget tree called multiple time

Solution
----

- Sink (Input) and Stream(Output) concept in StreamBuilder
- Bussiness logic to Bloc Pattern

## BLoC Pattern with Flutter | State Management

### main_block.dart

```
import 'package:flutter/material.dart';
import 'package:mro/basics_of_flutter_bloc/counter_bloc.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'Flutter BLOC Pattern Demo'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  //int _counter = 0;
  final counterBlock = CounterBloc();

  @override
  Widget build(BuildContext context) {
    print("---- Widget Tree ----");
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            StreamBuilder(
              stream: counterBlock.counterStream,
              initialData: 0,
              builder: (context, snapshot) {
                return Text(
                  '${snapshot.data}',
                  style: Theme.of(context).textTheme.headlineMedium,
                );
              },
            ),
          ],
        ),
      ),
      floatingActionButton: Row(
          mainAxisAlignment:MainAxisAlignment.spaceEvenly,
          children: [
        FloatingActionButton(
          onPressed: () {
            // _counter++;
            // counterBlock.counterSink.add(_counter);
            counterBlock.eventSink.add(CounterAction.Increment);
          },
          tooltip: 'Increment',
          child: const Icon(Icons.add),
        ),
        FloatingActionButton(
          onPressed: () {
            // _counter = 0;
            // counterBlock.counterSink.add(_counter);
            counterBlock.eventSink.add(CounterAction.Reset);
          },
          tooltip: 'Reset',
          child: const Icon(Icons.refresh),
        ),
        FloatingActionButton(
          onPressed: () {
            // _counter--;
            // counterBlock.counterSink.add(_counter);
            counterBlock.eventSink.add(CounterAction.Decrement);
          },
          tooltip: 'Decrement',
          child: const Icon(Icons.remove),
        ),
      ]),
    );
  }
}

```

### counter_block.dart

```
import 'dart:async';

// Basics of Bloc
enum CounterAction { Increment, Decrement, Reset }

class CounterBloc {
  int counter = 0;

  // OUTPUT STREAM CONTROLLER
  final _stateStreamController = StreamController<int>(); // Stream Controller
  StreamSink<int> get counterSink => _stateStreamController.sink; // Input
  Stream<int> get counterStream => _stateStreamController.stream; // Output

  // INPUT STREAM CONTROLLER : USER INPUT
  final _eventStreamController =
      StreamController<CounterAction>(); // Stream Controller
  StreamSink<CounterAction> get eventSink =>
      _eventStreamController.sink; // Input
  Stream<CounterAction> get eventStream =>
      _eventStreamController.stream; // Output

  // CONSTRUCTOR
  CounterBloc() {
    counter = 0;
    eventStream.listen((event) {
      if (event == CounterAction.Increment) {
        counter++;
      } else if (event == CounterAction.Decrement) {
        counter--;
      } else if (event == CounterAction.Reset) {
        counter = 0;
      }
      //  SENDING OUTPUT OF EVENT STREAM CONTROLLER TO FIRST STATE STREAM CONTROLLER AS INPUT
      counterSink.add(counter);
    });
  }
}

```
### What this code do?
- Take User Input : Increment or Decrement or Reset
- Make counter Increase or Decrease or Reset and show value on Text

### Output Stream Controller

![OUTPUT_Stream_Controller](https://github.com/NrupParikh/BasicsOfBlocInFlutter/assets/108717119/1c475153-f5db-4dc7-87fd-cbdb2ca809f1)


### Input Event Strean Controller

![INPUT_event_stream_controller](https://github.com/NrupParikh/BasicsOfBlocInFlutter/assets/108717119/d4be4e68-5b27-4eaf-83a5-c992c255637a)

### Reference

https://www.youtube.com/watch?v=jIoWkct6_EM
