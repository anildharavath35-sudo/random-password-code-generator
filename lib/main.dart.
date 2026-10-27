import 'dart:convert';
import 'package:crypto/crypto.dart';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

void main() {
  runApp(const PasswordCodeApp());
}

class PasswordCodeApp extends StatelessWidget {
  const PasswordCodeApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Random Password Code Generator',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepPurple),
      ),
      home: const PasswordCodePage(),
    );
  }
}

class PasswordCodePage extends StatefulWidget {
  const PasswordCodePage({super.key});

  @override
  State<PasswordCodePage> createState() => _PasswordCodePageState();
}

class _PasswordCodePageState extends State<PasswordCodePage> {
  final TextEditingController passwordController = TextEditingController();
  String generatedCode = '';
  bool hidePassword = true;
  String? errorMessage;

  void generateCode() {
    final password = passwordController.text;
    if (password.isEmpty) {
      setState(() {
        generatedCode = '';
        errorMessage = 'Please enter a password.';
      });
      return;
    }

    // Deterministic code: the same password generates the same result.
    final digest = sha256.convert(utf8.encode(password)).toString();
    final code = '${digest.substring(0, 4).toUpperCase()}-'
        '${digest.substring(4, 8).toUpperCase()}-'
        '${digest.substring(8, 12).toUpperCase()}';

    setState(() {
      generatedCode = code;
      errorMessage = null;
    });
  }

  Future<void> copyCode() async {
    if (generatedCode.isEmpty) return;
    await Clipboard.setData(ClipboardData(text: generatedCode));
    if (!mounted) return;
    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(content: Text('Generated code copied')),
    );
  }

  void clearFields() {
    passwordController.clear();
    setState(() {
      generatedCode = '';
      errorMessage = null;
    });
  }

  @override
  void dispose() {
    passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Random Password Code Generator'),
        centerTitle: true,
      ),
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(24),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              const SizedBox(height: 20),
              const Icon(Icons.lock_outline, size: 76, color: Colors.deepPurple),
              const SizedBox(height: 18),
              const Text(
                'Generate a Code from Your Password',
                textAlign: TextAlign.center,
                style: TextStyle(fontSize: 23, fontWeight: FontWeight.bold),
              ),
              const SizedBox(height: 10),
              const Text(
                'Enter a password to generate a repeatable code on this device.',
                textAlign: TextAlign.center,
              ),
              const SizedBox(height: 28),
              TextField(
                controller: passwordController,
                obscureText: hidePassword,
                autocorrect: false,
                enableSuggestions: false,
                decoration: InputDecoration(
                  labelText: 'Enter password',
                  border: const OutlineInputBorder(),
                  prefixIcon: const Icon(Icons.password),
                  errorText: errorMessage,
                  suffixIcon: IconButton(
                    tooltip: hidePassword ? 'Show password' : 'Hide password',
                    icon: Icon(
                      hidePassword ? Icons.visibility : Icons.visibility_off,
                    ),
                    onPressed: () {
                      setState(() => hidePassword = !hidePassword);
                    },
                  ),
                ),
                onSubmitted: (_) => generateCode(),
              ),
              const SizedBox(height: 18),
              FilledButton.icon(
                onPressed: generateCode,
                icon: const Icon(Icons.auto_awesome),
                label: const Text('Generate Code'),
                style: FilledButton.styleFrom(
                  padding: const EdgeInsets.symmetric(vertical: 16),
                ),
              ),
              const SizedBox(height: 10),
              OutlinedButton.icon(
                onPressed: clearFields,
                icon: const Icon(Icons.clear),
                label: const Text('Clear'),
              ),
              if (generatedCode.isNotEmpty) ...[
                const SizedBox(height: 28),
                Card(
                  child: Padding(
                    padding: const EdgeInsets.all(20),
                    child: Column(
                      children: [
                        const Text(
                          'Generated Code',
                          style: TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 18),
                        SelectableText(
                          generatedCode,
                          textAlign: TextAlign.center,
                          style: const TextStyle(
                            fontSize: 24,
                            fontWeight: FontWeight.bold,
                            letterSpacing: 2,
                          ),
                        ),
                        const SizedBox(height: 18),
                        FilledButton.tonalIcon(
                          onPressed: copyCode,
                          icon: const Icon(Icons.copy),
                          label: const Text('Copy Code'),
                        ),
                      ],
                    ),
                  ),
                ),
              ],
              const SizedBox(height: 22),
              const Text(
                'Privacy note: the password is processed locally and is not sent to a server. '
                'Do not use this app to derive codes from important account passwords. '
                'This demo uses an unsalted SHA-256 hash and is not an authentication or OTP system.',
                textAlign: TextAlign.center,
                style: TextStyle(fontSize: 12),
              ),
            ],
          ),
        ),
      ),
    );
  }
}
