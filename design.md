# Design Document for Linkarooie

## Introduction

Linkarooie revolutionizes the way users manage their online presence, enabling a unified and professional digital identity across various platforms.

## System Overview

Linkarooie allows users to upload and synchronize their profile images across multiple online services, streamlining the process of maintaining consistent avatars and banners.

## Functional Requirements

1. **User Authentication**: Secure and straightforward user onboarding.
2. **Image Upload**: A central hub for image management.
3. **Image Processing**: Automated adjustments to fit platform specifications.
4. **API Integration**: Seamless connectivity with external platforms.
5. **User API Key and Token Management**: Efficient handling of user credentials.

## Technical Architecture

```mermaid
graph LR
    A[User Interface] --> B[User Authentication]
    A --> C[Image Upload and Processing]
    B --> D[Database]
    C --> D
    C --> E[API Integration with OAuth]
    E --> F{External Platforms}
    D -.-> E
    E --> G[AWS Services]
```

## Components

1. **User Interface**: Intuitive and user-friendly.
2. **User Authentication**: Robust security with Devise.
3. **Image Upload and Processing**: Streamlined media handling.
4. **API Integration with OAuth**: Authentic and secure third-party interactions.
5. **Database**: Reliable data storage on AWS RDS.
6. **AWS Services**: Scalable infrastructure utilizing ECS, Fargate, S3, and RDS.

## AWS Architecture

```mermaid
graph LR
    A[User Interface] -->|HTTP/HTTPS| B[Load Balancer]
    B -->|Docker Containers| C[ECS Fargate]
    C -->|Connects| D[RDS - PostgreSQL]
    C -->|Stores| E[S3 for Images]
```

## Database Schema

```mermaid
erDiagram
    USER ||--o{ OAUTH_CREDENTIALS : has
    USER ||--o{ IMAGES : uploads
    USER {
        string email
        string encrypted_password
    }
    OAUTH_CREDENTIALS {
        string platform_name
        string access_token
        string refresh_token
        datetime expires_at
    }
    IMAGES {
        string image_file
    }
```

## API Integration Flow

```mermaid
sequenceDiagram
    actor U as User
    participant UI as User Interface
    participant S as Server
    participant O as OAuth Service
    participant API as External API

    U->>UI: Choose platform and initiate connection
    UI->>S: Request OAuth link
    S->>O: Request authorization URL
    O->>U: Redirect for authentication
    U->>O: Log in and approve access
    O->>S: Redirect with access token
    S->>UI: Confirm connection
    U->>UI: Upload image
    UI->>S: Process image
    S->>API: Update image using access token
    API-->>S: Confirm update
    S-->>UI: Show update status
    UI-->>U: Display result
```

## Development and Deployment

* **Ruby and Rails**: Stay current with the latest stable releases.
* **Deployment**: Leverage AWS ECS with Fargate for efficient container management, S3 for robust image storage, and RDS for a resilient PostgreSQL database.

> \[!NOTE] Ensure all images are processed to meet individual platform requirements, guaranteeing a seamless and consistent user experience.

## Conclusion

Linkarooie is not just an application, it's a commitment to empowering users to maintain a consistent and professional online image, effortlessly syncing their digital identities across the web's most popular platforms.
