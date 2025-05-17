# WebRTC Audio Call Server

This server provides a signaling service for WebRTC audio calls between users, with user authentication, call history tracking, and more.

## Features

- User authentication (register/login)
- Real-time communication via WebSockets (Socket.io)
- WebRTC signaling for audio calls
- Call history tracking
- Testing utilities

## Setup

1. Install dependencies:
   ```
   npm install
   ```

2. Create a `.env` file with the following variables:
   ```
   PORT=3000
   MONGO_URI=your_mongodb_connection_string
   JWT_SECRET=your_jwt_secret
   ```

3. Start the server:
   ```
   npm start
   ```

## WebRTC Audio Call Flow

The WebRTC audio call implementation follows these steps:

1. **Authentication**: Users must authenticate to get a JWT token
2. **Socket Connection**: Users connect to the WebSocket server with their token
3. **Call Initiation**: Caller sends an offer to the callee via the signaling server
4. **Call Response**: Callee accepts or rejects the call
5. **ICE Candidate Exchange**: Both peers exchange ICE candidates
6. **Media Connection**: WebRTC establishes a direct P2P connection for audio
7. **Call End**: Either user can end the call

## API Endpoints

### Authentication

- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Login and get a token

### Call History

- `GET /api/calls/history` - Get call history for authenticated user
- `GET /api/calls/details/:callId` - Get details for a specific call

### Testing

- `POST /api/calls/test-webrtc` - Test WebRTC connection between two users

## Testing WebRTC Connectivity

You can test WebRTC connectivity between two users using one of these methods:

### Method 1: Using Thunder Client

1. Import the `thunder-collection_webrtc-test.json` collection into Thunder Client
2. Register or login two test users using the Authentication requests
3. Run the "Run WebRTC Connection Test" request

### Method 2: Using the Command Line

Run the test directly from the command line:

```
npm run test:webrtc
```

### Method 3: Manual Testing

You can also test the connection manually by:

1. Creating two user accounts
2. Using the tokens to establish socket connections
3. Initiating a call from one user to the other
4. Checking the logs to verify call signaling works correctly

## Socket.io Events

### Client Events (Emitted by clients)

- `call-offer` - Initiates a call to another user
- `call-answer` - Responds to a call offer
- `call-ice-candidate` - Sends ICE candidate to the peer
- `call-end` - Ends an ongoing call

### Server Events (Listened by clients)

- `call-offer` - Receives a call offer
- `call-offer-sent` - Confirmation that an offer was sent
- `call-answer` - Receives a call answer (accept/reject)
- `call-ice-candidate` - Receives an ICE candidate from the peer
- `call-end` - Notifies that a call has ended

## Frontend Integration

After successfully testing the backend WebRTC implementation, you can integrate with the frontend React Native app by:

1. Implementing the WebRTC client using react-native-webrtc
2. Connecting to the Socket.io server
3. Implementing the call UI and logic
4. Handling permissions for audio access
5. Testing end-to-end call functionality 