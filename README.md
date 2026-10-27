# Random Password Code Generator

A Flutter demo app that accepts text in a password field and generates a repeatable code using SHA-256.

## Features

- Password input with show/hide control
- Generates a formatted 12-character code
- Copy generated code to clipboard
- Clear input and result
- Processes the entered text locally; no server connection

## Run locally

1. Install Flutter.
2. From this project directory, run:

   ```bash
   flutter pub get
   flutter run
   ```

## Important security note

This is a learning/demo project, not a password manager, authentication service, or OTP generator. The same input produces the same code. It uses an unsalted SHA-256 hash, which is not suitable for protecting real passwords. Do not enter passwords used for important accounts.
