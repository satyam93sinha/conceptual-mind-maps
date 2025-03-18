Here's a markdown mind map based on the Arpit Bhayani's RPC Youtube Video's transcript provided:

```markdown
# Remote Procedure Calls (RPCs)

## **Introduction**
- **Revival**: RPCs have seen a resurgence after a decade.
- **Purpose**: Facilitate inter-service communication over networks.

## **Core Concept**
- **RPC vs Local Calls**: 
  - **Definition**: Makes network calls appear like local function calls.
  - **Abstraction**: Hides complexity (serialization, decentralization, transport).

## **How RPC Works**
- **Client-Server Model**:
  - **Client**: Service requesting information.
  - **Server**: Service providing the response.
- **Communication Protocols**: Not limited to HTTP; can use TCP, UDP, etc.
  - **Common Misconception**: RPC ≠ HTTP or REST.

### **Stubs**
- **Role of Stubs**: 
  - **Marshalling**: Converts data types between client and server languages (e.g., Golang struct to Java class).
  - **Conversion**: Handles request and response serialization/deserialization.
- **Stub Generation**: 
  - **Interface Definition Language (IDL)**: Defines types and services.
  - **Stub Generators**: Tools like gRPC use protobuf to generate language-specific code.
  - **Example**: Using protobuf for defining services and messages.

### **Sample Code (Python)**
```python
from grpc import server, insecure_channel
from example_pb2 import SendNotificationRequest, SendNotificationResponse
from example_pb2_grpc import NotificationServiceStub, add_NotificationServiceServicer_to_server

# Server Side
class NotificationService:
    def SendOTP(self, request, context):
        # Business logic here
        return SendNotificationResponse(message=f"OTP sent to {request.email}")

def serve():
    server_instance = server()
    add_NotificationServiceServicer_to_server(NotificationService(), server_instance)
    server_instance.add_insecure_port('[::]:50051')
    server_instance.start()
    server_instance.wait_for_termination()

# Client Side
def client():
    with insecure_channel('localhost:50051') as channel:
        stub = NotificationServiceStub(channel)
        response = stub.SendOTP(SendNotificationRequest(email="example@example.com"))
    print(response.message)

if __name__ == '__main__':
    serve()  # Run server in another process or thread for testing
    client()
```

## **Advantages of RPC**
- **Simplicity**: Looks like local function calls, enhancing developer productivity.
- **Strong API Contracts**: Explicit interface definitions ensure clarity.
- **Language Agnostic**: Supports multiple languages via stub generation.
- **Performance**: Offers features like payload compression, connection pooling.
- **Security**: Can be abstracted at the RPC level.
- **Client Libraries**: Auto-generated, reducing maintenance overhead.

## **Challenges and Considerations**
- **Regeneration**: Stubs need updating when signatures change.
- **Testing**: More complex due to network calls.
- **Initial Setup**: More steps than REST to get started (IDL, stub generation).
- **Browser Support**: Not all browsers support RPC directly.

## **Implementation**
- **gRPC Example**: 
  - **protobuf**: Defines services and messages.
  - **Language Support**: Generates code for various languages.

## **Conclusion**
- **When to Use**: Ideal for complex, multi-language service communications.
- **Learning Curve**: Worth the effort for its benefits despite initial complexity.

## **Resources**
- **GitHub**: Suggested to check out a basic RPC example in Golang for practical understanding.
- **Recommendation**: Implement a simple "Hello World" with gRPC to grasp the concept.
