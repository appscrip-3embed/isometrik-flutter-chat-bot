# Chat Bot Package Usage Guide

## Overview
This Flutter package provides a complete chat bot solution with speech recognition capabilities.

## Installation

Add this package to your `pubspec.yaml`:

```yaml
dependencies:
  chat_bot:
    git:
      url: https://github.com/your-repo/isometrik-flutter-chat-bot.git
      ref: chat_history  # or your desired branch
```

## Dependencies

Make sure your consuming project includes these dependencies in `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  speech_to_text: ^6.6.0  # Required for speech recognition
  flutter_bloc: ^8.1.3
  bloc: ^8.1.2
  shared_preferences: ^2.2.2
  http: ^1.1.0
  lottie: ^2.7.0
  unique_identifier: ^0.4.0
  flutter_svg: ^2.2.0
  shimmer: ^3.0.0
```

## Usage

### Basic Setup

```dart
import 'package:chat_bot/chat_bot.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Chat Bot Demo')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Configure the chat bot
            ChatBot.configure(
              chatBotId: 'your-chat-bot-id',
              appSecret: 'your-app-secret',
              licenseKey: 'your-license-key',
              isProduction: false,
              userId: 'user123',
              name: 'John Doe',
              timestamp: DateTime.now().millisecondsSinceEpoch.toString(),
              userToken: 'user-token',
              location: 'New York',
              longitude: -74.006,
              latitude: 40.7128,
              needToShowTutorial: false,
              clientGuid: 'client-guid',
              indexName: 'index-name',
              visitId: 'visit-id',
              visitorId: 'visitor-id',
              searchApiUrl: 'https://your-search-api-url.com',
            );
            
            // Open the chat bot
            ChatBot.openChatBot(context);
          },
          child: Text('Open Chat Bot'),
        ),
      ),
    );
  }
}
```

### Using Speech Service

```dart
import 'package:chat_bot/chat_bot.dart';

class MyWidget extends StatefulWidget {
  @override
  _MyWidgetState createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> {
  final SpeechService _speechService = SpeechService();
  String _recognizedText = '';

  @override
  void initState() {
    super.initState();
    _initializeSpeech();
  }

  Future<void> _initializeSpeech() async {
    await _speechService.initialize();
  }

  void _startListening() {
    if (_speechService.startListening()) {
      print('Started listening');
    }
  }

  void _stopListening() {
    _speechService.stopListening();
  }

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        Text('Recognized: $_recognizedText'),
        ElevatedButton(
          onPressed: _startListening,
          child: Text('Start Listening'),
        ),
        ElevatedButton(
          onPressed: _stopListening,
          child: Text('Stop Listening'),
        ),
      ],
    );
  }
}
```

## Troubleshooting

### Common Issues

1. **Compilation Errors with Speech Service**
   - Ensure `speech_to_text: ^6.6.0` is in your dependencies
   - Run `flutter pub get` to resolve dependencies
   - Clean and rebuild: `flutter clean && flutter pub get`

2. **Permission Issues**
   - Add microphone permissions to your Android manifest:
   ```xml
   <uses-permission android:name="android.permission.RECORD_AUDIO" />
   ```
   - Add microphone permissions to your iOS Info.plist:
   ```xml
   <key>NSMicrophoneUsageDescription</key>
   <string>This app needs access to microphone for speech recognition</string>
   ```

3. **Package Resolution Issues**
   - Make sure you're using the correct branch/commit
   - Check that all dependencies are compatible
   - Try running `flutter pub deps` to see dependency tree

### Android Specific Setup

Add to your `android/app/build.gradle`:

```gradle
android {
    compileSdkVersion 34
    
    defaultConfig {
        minSdkVersion 21
        targetSdkVersion 34
    }
}
```

### iOS Specific Setup

Add to your `ios/Runner/Info.plist`:

```xml
<key>NSMicrophoneUsageDescription</key>
<string>This app needs access to microphone for speech recognition</string>
```

## API Reference

### ChatBot Class

#### Static Methods

- `configure({...})` - Configure the chat bot with required parameters
- `openChatBot(BuildContext context)` - Open the chat bot interface
- `isCartUpdate(dynamic cartData)` - Handle cart update callbacks
- `openStripePayment(String cartNumber)` - Open Stripe payment
- `openAddressSummary(String addressSummary)` - Open address summary

### SpeechService Class

#### Methods

- `initialize()` - Initialize speech recognition
- `startListening()` - Start listening for speech
- `stopListening()` - Stop listening
- `cancel()` - Cancel speech recognition
- `getAvailableLocales()` - Get available speech locales
- `isSupported()` - Check if speech recognition is supported
- `requestPermission()` - Request microphone permission

#### Properties

- `isAvailable` - Whether speech recognition is available
- `isListening` - Whether currently listening
- `currentRecognizedText` - Current recognized text

## Support

For issues and questions, please check the troubleshooting section above or contact support.

