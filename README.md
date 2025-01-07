# EmoStream: Concurrent Emoji Broadcast over Event-Driven Architecture

## Project Overview
EmoStream captures and broadcasts the dynamic sentiments of fans in real time during live events. By processing user-generated emoji reactions, this project creates an emoji swarm that mirrors the collective mood of the audience, enhancing the viewer experience on platforms like Hotstar.

The system leverages an event-driven architecture to handle high concurrency and low latency, ensuring seamless processing and broadcasting of emoji data.

---

## Features
- **Real-Time Emoji Processing**: Handles emoji reactions from up to 100 concurrent users.
- **Scalable Architecture**: Designed to process large volumes of data efficiently.
- **Enhanced Viewer Experience**: Aggregates emoji data to reflect live audience sentiment.

---

## Technologies Used
- **Backend Framework**: Flask (Python)
- **Message Queuing**: Apache Kafka
- **Data Processing**: Apache Spark (6-second window for streaming)
- **Testing Tools**: JMeter, Postman
- **Architecture**: Pub-Sub Model

---

## Application Architecture

### 1. Handling Client Requests
- **API Endpoint**:
  - Flask HTTP endpoint receives emoji data (`User ID`, `Emoji Type`, `Timestamp`).
  - Data is asynchronously written to a Kafka producer.
- **Kafka Producer**:
  - Buffers emoji data and sends it to the Kafka broker every 500 milliseconds.

### 2. Processing Data Streams
- **Spark Streaming**:
  - Consumes emoji data from the Kafka broker.
  - Processes data in 6-second micro-batches.
  - Aggregates up to 10 emojis of the same type into a single representative count.

### 3. Delivering Processed Messages
- **Pub-Sub Architecture**:
  - **Main Publisher**: Routes processed data to cluster publishers.
  - **Clusters**: Each cluster has a publisher and multiple subscribers.
  - **Subscribers**: Deliver aggregated emoji data to clients in real time.
- **Client Registration**:
  - API for clients to register and receive real-time data streaming.
  - Seamless addition and removal of users during live processing.

---

## Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/EmoStream.git
   cd EmoStream
   ```
2. Create a virtual environment:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Set up Kafka:
   - Install and start Apache Kafka.
   - Update Kafka broker settings in the Flask configuration.
5. Start the Spark streaming job:
   - Ensure Spark is installed.
6. Follow the commands given in the commands.txt file

---

## Usage
1. **Send Emoji Data:**
   - Use a tool like Postman to send POST requests to the API endpoint.
     ```json
     {
         "user_id": "123",
         "emoji_type": "😊",
         "timestamp": "2025-01-07T12:00:00Z"
     }
     ```
2. **Simulate High Load:**
   - Use threading or load testing tools to simulate 100 concurrent users.

3. **View Aggregated Data:**
   - Aggregated emoji data is broadcast to subscribers in real time.

---

## Testing
- **Unit Tests:** Validate API endpoints under high emoji submission rates.
- **Load Testing:** Use JMeter to test concurrency with up to 100 users.

---

## Future Scope
- **Sentiment Analysis**: Integrate sentiment analysis to provide deeper insights into audience reactions.
- **Extended Data Types**: Support non-emoji reactions like text and GIFs.
- **Machine Learning**: Use predictive analytics for audience behavior forecasting.
- 
---

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

