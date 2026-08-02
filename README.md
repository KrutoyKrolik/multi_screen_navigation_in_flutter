# multi_screen_navigation_in_flutter

import 'package:flutter/material.dart';

void main() {
  runApp(const MultiScreenApp());
}

class MultiScreenApp extends StatelessWidget {
  const MultiScreenApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Multi-Screen Navigation',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.teal),
        useMaterial3: true,
      ),
      home: const HomeScreen(),
    );
  }
}

// ==========================================
// SCREEN 1: Home Screen
// ==========================================
class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  String? _resultFromDetails;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Home Screen (Screen 1)')),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              const Text(
                'Welcome to the Multi-Screen App!',
                style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 20),
              ElevatedButton.icon(
                icon: const Icon(Icons.arrow_forward),
                label: const Text('Go to Form Screen'),
                onPressed: () {
                  Navigator.push(
                    context,
                    MaterialPageRoute(
                      builder: (context) => const FormScreen(),
                    ),
                  );
                },
              ),
              const SizedBox(height: 30),
              if (_resultFromDetails != null)
                Container(
                  padding: const EdgeInsets.all(12),
                  decoration: BoxDecoration(
                    color: Colors.teal.shade50,
                    borderRadius: BorderRadius.circular(8),
                    border: Border.all(color: Colors.teal),
                  ),
                  child: Text(
                    'Data returned from Screen 3:\n"$_resultFromDetails"',
                    textAlign: TextAlign.center,
                    style: const TextStyle(fontSize: 16),
                  ),
                ),
            ],
          ),
        ),
      ),
    );
  }
}

// ==========================================
// SCREEN 2: Form Screen (Sends Data Forward)
// ==========================================
class FormScreen extends StatefulWidget {
  const FormScreen({super.key});

  @override
  State<FormScreen> createState() => _FormScreenState();
}

class _FormScreenState extends State<FormScreen> {
  final TextEditingController _controller = TextEditingController();

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Form Screen (Screen 2)')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _controller,
              decoration: const InputDecoration(
                labelText: 'Enter a custom message',
                border: OutlineInputBorder(),
              ),
            ),
            const SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                final message = _controller.text.isEmpty
                    ? 'Default Message'
                    : _controller.text;

                // Pass data forward via constructor
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => DetailsScreen(userMessage: message),
                  ),
                );
              },
              child: const Text('Pass Data to Details Screen'),
            ),
          ],
        ),
      ),
    );
  }
}

// ==========================================
// SCREEN 3: Details Screen (Returns Data Back)
// ==========================================
class DetailsScreen extends StatelessWidget {
  final String userMessage;

  const DetailsScreen({super.key, required this.userMessage});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Details Screen (Screen 3)')),
      body: Center(
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Text(
                'Received from Screen 2:\n"$userMessage"',
                textAlign: TextAlign.center,
                style: const TextStyle(fontSize: 20, fontWeight: FontWeight.w500),
              ),
              const SizedBox(height: 30),
              ElevatedButton.icon(
                icon: const Icon(Icons.check_circle),
                label: const Text('Complete & Pass Data Back to Home'),
                onPressed: () async {
                  // Pop back to HomeScreen and pass result
                  Navigator.popUntil(context, (route) => route.isFirst);
                  
                  // Alternatively, to pop one screen and pass back:
                  // Navigator.pop(context, 'Confirmed: $userMessage');
                },
              ),
            ],
          ),
        ),
      ),
    );
  }
}
