# Implementation Plan: Kisan-Drishti (Farmer's Vision)

## Overview

This implementation plan breaks down the Kisan-Drishti system into discrete coding tasks. The approach follows a bottom-up strategy: first establishing core components (data models, robot controller), then building the AI/ML layer (speech-to-intent, mission planning), followed by orchestration (n8n workflows), and finally integration testing. Each task builds incrementally to ensure continuous validation.

## Tasks

- [ ] 1. Set up project structure and core data models
  - Create Python project with virtual environment
  - Define data models: Intent, Target, Location, MissionPlan, Phase, ActuatorCommand, RobotState, Session, Response
  - Implement serialization/deserialization for all models
  - Set up testing framework (pytest, hypothesis)
  - _Requirements: 1.5, 2.5, 3.5, 10.1, 10.2_

- [ ]* 1.1 Write property test for data model serialization
  - **Property: Serialization round trip**
  - **Validates: Requirements 1.5**

- [ ] 2. Implement MuJoCo robot controller
  - [ ] 2.1 Create RobotController class with MuJoCo integration
    - Load Hello Robot Stretch 2 model from Menagerie
    - Implement actuator mapping (0-7 for all DOF)
    - Implement get_state() to return current robot configuration
    - _Requirements: 3.1, 3.2, 9.1, 9.2, 9.3, 9.4, 9.5, 9.6_

  - [ ]* 2.2 Write property test for actuator mapping correctness
    - **Property 8: Actuator Mapping Correctness**
    - **Validates: Requirements 3.2, 9.1-9.6**

  - [ ] 2.3 Implement weld constraint (Magnet Pick) logic
    - Create apply_weld_constraint() method
    - Test with simple grasp scenarios
    - _Requirements: 3.3_

  - [ ]* 2.4 Write property test for weld constraint application
    - **Property 9: Weld Constraint Application**
    - **Validates: Requirements 3.3**

  - [ ] 2.5 Implement collision detection and reporting
    - Add real-time physics validation during trajectory execution
    - Return collision details when detected
    - _Requirements: 3.4_

  - [ ]* 2.6 Write property test for collision detection
    - **Property 10: Collision Detection and Reporting**
    - **Validates: Requirements 3.4**

  - [ ] 2.7 Implement joint limit enforcement
    - Add constraint checking for all actuators
    - Clamp values that violate limits
    - _Requirements: 9.7_

  - [ ]* 2.8 Write property test for joint limit enforcement
    - **Property 27: Joint Limit Enforcement**
    - **Validates: Requirements 9.7**

  - [ ] 2.9 Implement head camera image capture
    - Add capture_camera_image() method
    - Return RGB numpy array from head camera
    - _Requirements: 4.1_

- [ ] 3. Checkpoint - Ensure robot controller tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 4. Implement Speech-to-Intent Engine
  - [ ] 4.1 Create SpeechToIntentEngine class
    - Integrate Google Speech-to-Text API
    - Implement detect_language() for Indian languages (Hindi, Marathi, Tamil, Telugu, Bengali, Gujarati)
    - Implement transcribe() method
    - _Requirements: 1.2, 7.1_

  - [ ]* 4.2 Write unit tests for language detection
    - Test with sample audio in each supported language
    - _Requirements: 1.2_

  - [ ] 4.3 Implement intent extraction using Gemini
    - Create extract_intent() method using Gemini API
    - Parse transcript into structured Intent object
    - Calculate confidence scores
    - _Requirements: 1.3, 1.5_

  - [ ]* 4.4 Write property test for intent extraction completeness
    - **Property 2: Intent Extraction Completeness**
    - **Validates: Requirements 1.3, 1.5**

  - [ ] 4.5 Implement clarification logic for unclear input
    - Detect low confidence or unsupported language
    - Generate clarification request in detected language
    - _Requirements: 1.4_

  - [ ]* 4.6 Write property test for clarification on invalid input
    - **Property 3: Clarification on Invalid Input**
    - **Validates: Requirements 1.4**

- [ ] 5. Implement Google Antigravity Agent integration
  - [ ] 5.1 Create AntigravityAgent class
    - Integrate with Google Antigravity API
    - Implement plan_mission() to generate trajectory plans
    - _Requirements: 2.1, 2.2_

  - [ ]* 5.2 Write property test for mission planning latency
    - **Property 4: Mission Planning Latency**
    - **Validates: Requirements 2.1**

  - [ ] 5.3 Implement pick operation phase generation
    - Ensure all pick operations include approach, grasp, retract phases
    - _Requirements: 2.3_

  - [ ]* 5.4 Write property test for pick operation phase completeness
    - **Property 5: Pick Operation Phase Completeness**
    - **Validates: Requirements 2.3**

  - [ ] 5.5 Implement unreachable target detection
    - Check workspace constraints
    - Generate user-friendly explanations and alternatives
    - _Requirements: 2.4_

  - [ ]* 5.6 Write property test for unreachable target notification
    - **Property 6: Unreachable Target Notification**
    - **Validates: Requirements 2.4**

  - [ ] 5.7 Implement Python code generation for MuJoCo
    - Generate executable Python scripts from mission plans
    - Validate syntax before returning
    - _Requirements: 2.5_

  - [ ]* 5.8 Write property test for Python code generation validity
    - **Property 7: Python Code Generation Validity**
    - **Validates: Requirements 2.5**

- [ ] 6. Checkpoint - Ensure AI components tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 7. Implement Feedback Generator
  - [ ] 7.1 Create FeedbackGenerator class
    - Implement capture_and_annotate() to overlay text on images
    - Support Unicode fonts for Devanagari and other scripts
    - _Requirements: 4.2, 4.3_

  - [ ]* 7.2 Write property test for feedback image capture
    - **Property 12: Feedback Image Capture**
    - **Validates: Requirements 4.1, 4.2**

  - [ ]* 7.3 Write property test for feedback language consistency
    - **Property 13: Feedback Language Consistency**
    - **Validates: Requirements 4.3**

  - [ ] 7.4 Implement voice message generation
    - Integrate text-to-speech for supported languages
    - Generate audio confirmation messages
    - _Requirements: 4.5_

  - [ ] 7.5 Implement image compression for WhatsApp
    - Compress images to meet WhatsApp size limits
    - Maintain visual quality
    - _Requirements: 4.4_

  - [ ]* 7.6 Write property test for feedback completeness
    - **Property 15: Feedback Completeness**
    - **Validates: Requirements 4.5**

- [ ] 8. Implement WhatsApp Gateway
  - [ ] 8.1 Create WhatsAppGateway class
    - Integrate Wassenger or Twilio API
    - Implement receive_message() for incoming voice notes
    - Support AAC, MP3, OGG audio formats
    - _Requirements: 5.1, 5.2_

  - [ ]* 8.2 Write unit tests for audio format support
    - Test with sample files in each format
    - _Requirements: 5.2_

  - [ ] 8.3 Implement authentication and permission validation
    - Validate sender phone numbers
    - Check user permissions
    - _Requirements: 5.1_

  - [ ]* 8.4 Write property test for user authentication
    - **Property 16: User Authentication**
    - **Validates: Requirements 5.1**

  - [ ] 8.5 Implement send methods (text, media, voice)
    - Create send_text(), send_media(), send_voice() methods
    - Handle API rate limits
    - _Requirements: 5.3_

  - [ ] 8.6 Implement retry logic with exponential backoff
    - Add connection failure handling
    - Retry up to 3 times with exponential backoff
    - _Requirements: 5.4_

  - [ ]* 8.7 Write property test for connection retry
    - **Property 17: Connection Retry with Exponential Backoff**
    - **Validates: Requirements 5.4**

  - [ ] 8.8 Implement session management
    - Create get_session() method
    - Maintain conversation context per phone number
    - _Requirements: 5.5, 10.1, 10.2_

  - [ ]* 8.9 Write property test for session context maintenance
    - **Property 18: Session Context Maintenance**
    - **Validates: Requirements 5.5, 10.2**

  - [ ]* 8.10 Write property test for unique session creation
    - **Property 28: Unique Session Creation**
    - **Validates: Requirements 10.1**

- [ ] 9. Implement session and context management
  - [ ] 9.1 Create Session class with context tracking
    - Track conversation history, robot state, picked objects
    - Implement context-based inference for follow-up commands
    - _Requirements: 10.2, 10.3, 10.5_

  - [ ]* 9.2 Write property test for context-based intent inference
    - **Property 29: Context-Based Intent Inference**
    - **Validates: Requirements 10.3, 10.5**

  - [ ] 9.3 Implement session timeout and cleanup
    - Close sessions after 10 minutes of inactivity
    - Clear context on session close
    - _Requirements: 10.4_

  - [ ]* 9.4 Write property test for session timeout
    - **Property 30: Session Timeout and Cleanup**
    - **Validates: Requirements 10.4**

  - [ ] 9.5 Implement language detection and persistence
    - Detect language on first interaction
    - Persist preference for future interactions
    - _Requirements: 7.1, 7.2, 7.3_

  - [ ]* 9.6 Write property test for language detection and persistence
    - **Property 24: Language Detection and Persistence**
    - **Validates: Requirements 7.1, 7.2, 7.3**

  - [ ] 9.7 Implement low confidence language confirmation
    - Ask for confirmation when confidence < 70%
    - _Requirements: 7.5_

  - [ ]* 9.8 Write property test for low confidence language confirmation
    - **Property 25: Low Confidence Language Confirmation**
    - **Validates: Requirements 7.5**

- [ ] 10. Checkpoint - Ensure session management tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 11. Implement n8n orchestration workflows
  - [ ] 11.1 Create n8n workflow for voice command processing
    - Set up webhook trigger for WhatsApp messages
    - Add nodes for each pipeline stage (Speech-to-Intent → Antigravity → Robot Controller → Feedback)
    - Configure error handling and retries
    - _Requirements: 6.1_

  - [ ] 11.2 Implement comprehensive logging
    - Log all API calls, responses, timestamps, latency
    - Store logs in structured format
    - _Requirements: 6.2_

  - [ ]* 11.3 Write property test for comprehensive logging
    - **Property 20: Comprehensive Logging**
    - **Validates: Requirements 6.2**

  - [ ] 11.3 Implement error handling with notifications
    - Capture component failures
    - Send admin alerts
    - Generate user-friendly error messages in farmer's language
    - _Requirements: 6.3, 8.1, 8.2, 8.3, 8.5_

  - [ ]* 11.4 Write property test for error handling
    - **Property 21: Error Handling with User Notification**
    - **Validates: Requirements 6.3, 8.1, 8.2, 8.3, 8.5**

  - [ ] 11.5 Implement latency monitoring and enforcement
    - Track end-to-end latency
    - Alert if exceeding 15 second SLA
    - _Requirements: 6.4_

  - [ ]* 11.6 Write property test for end-to-end latency
    - **Property 22: End-to-End Latency Enforcement**
    - **Validates: Requirements 6.4**

  - [ ] 11.7 Implement metrics exposure
    - Expose request count, success rate, average latency
    - Create monitoring dashboard endpoint
    - _Requirements: 6.5_

  - [ ]* 11.8 Write property test for metrics exposure
    - **Property 23: Metrics Exposure**
    - **Validates: Requirements 6.5**

  - [ ] 11.9 Implement timeout status updates
    - Send status messages when components timeout
    - Keep farmer informed of processing status
    - _Requirements: 8.4_

  - [ ]* 11.10 Write property test for timeout status updates
    - **Property 26: Timeout Status Updates**
    - **Validates: Requirements 8.4**

- [ ] 12. Implement Python API endpoints for n8n integration
  - [ ] 12.1 Create Flask/FastAPI server
    - Set up REST API endpoints for each component
    - Implement /speech-to-intent, /plan-mission, /execute-robot, /generate-feedback
    - Add request validation and error handling
    - _Requirements: 6.1_

  - [ ] 12.2 Implement latency tracking middleware
    - Measure and log request processing time
    - Add latency headers to responses
    - _Requirements: 1.1, 4.4_

  - [ ]* 12.3 Write property test for voice interface latency
    - **Property 1: Voice Interface Latency**
    - **Validates: Requirements 1.1**

  - [ ]* 12.4 Write property test for feedback delivery latency
    - **Property 14: Feedback Delivery Latency**
    - **Validates: Requirements 4.4**

- [ ] 13. Integration and end-to-end wiring
  - [ ] 13.1 Wire all components together
    - Connect WhatsApp Gateway → n8n → Python APIs → MuJoCo
    - Test complete pipeline with sample voice note
    - _Requirements: 6.1_

  - [ ]* 13.2 Write property test for pipeline orchestration
    - **Property 19: Pipeline Orchestration Completeness**
    - **Validates: Requirements 6.1**

  - [ ] 13.3 Create deployment configuration
    - Set up Ubuntu server environment
    - Configure n8n, Python services, MuJoCo
    - Document environment variables and secrets
    - _Requirements: 6.1_

  - [ ] 13.4 Implement graceful degradation
    - Add fallback for Antigravity Agent unavailability
    - Handle MuJoCo simulation failures
    - Fallback to text-only if TTS fails
    - _Requirements: 8.1, 8.2, 8.3_

- [ ] 14. Create test data and simulation scenarios
  - [ ] 14.1 Create voice note corpus
    - Collect sample voice notes in all supported languages
    - Include variations: accents, background noise, speaking speeds
    - Cover all command types
    - _Requirements: 1.2, 1.3_

  - [ ] 14.2 Create MuJoCo simulation scenes
    - Build strawberry field scene with varying densities
    - Add obstacles (leaves, branches)
    - Create edge case scenarios (empty field, unreachable locations)
    - _Requirements: 3.1, 3.4_

  - [ ] 14.3 Create translation test data
    - Document culturally appropriate terminology for each language
    - Create test cases for technical term translation
    - _Requirements: 7.4_

- [ ] 15. Final checkpoint - End-to-end testing
  - Run complete pipeline with test data
  - Verify all properties hold
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation at key milestones
- Property tests validate universal correctness properties with minimum 100 iterations each
- Unit tests validate specific examples, edge cases, and integration points
- The implementation uses Python for all backend components
- n8n workflows are configured through the web UI and exported as JSON
- MuJoCo simulation runs locally on Ubuntu server
