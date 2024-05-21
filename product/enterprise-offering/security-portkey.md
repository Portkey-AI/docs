# Security @ Portkey

Portkey AI provides a secure, reliable AI gateway for the seamless integration and management of large language models (LLMs).&#x20;

This document outlines our robust security protocols, designed to meet the needs of businesses requiring high standards of data protection and operational reliability.&#x20;

At Portkey AI, we understand the critical importance of security in today's digital landscape and are committed to delivering state-of-the-art solutions that protect our clients' data and ensure their AI applications run smoothly.

## Security Framework

### **Authentication**

Portkey AI ensures secure API access through token-based authentication mechanisms, supporting Single Sign-On (SSO) via OIDC on enterprise plans.&#x20;

We also implement [Virtual Keys](../ai-gateway-streamline-llm-integrations/virtual-keys/), which provide an added layer of security by securely storing provider API keys within a controlled and monitored environment.&#x20;

This multi-tier authentication strategy is crucial for protecting against unauthorized access and ensuring the integrity of user interactions.

### Encryption

To protect data integrity and privacy, all data in transit to and from Portkey AI is encrypted using TLS 1.2 or higher. We employ AES-256 encryption for data at rest, safeguarding it against unauthorized access and breaches.&#x20;

These encryption standards are part of our commitment to maintaining secure data channels and storage.

### Access Control

Our access control measures are designed with granularity to offer precise control over who can see and manage data.&#x20;

For enterprise clients, we provide enhanced [Role-Based Access Control (RBAC)](access-control-management.md#id-2.-fine-grained-user-roles-and-permissions), allowing for stringent governance suited to complex organizational needs.&#x20;

This system is pivotal for enforcing security policies and ensuring that only authorized personnel have access to sensitive operations and data.

## Compliance and Data Privacy

### **Compliance with Standards**

Our platform is compliant with leading security standards, including [SOC2, ISO27001, GDPR, and HIPAA](https://trust.portkey.ai).&#x20;

Portkey AI undergoes regular audits, compliance checks, and penetration testing conducted by third-party security experts to ensure continuous adherence to these standards.&#x20;

These certifications demonstrate our commitment to global security practices and our ability to meet diverse regulatory requirements.

<figure><img src="../../.gitbook/assets/Untitled design (14).png" alt=""><figcaption></figcaption></figure>

### **Privacy Protections**

At Portkey AI, we prioritize user privacy. Our privacy protocols are designed to comply with international data protection regulations, ensuring that all data is handled responsibly.&#x20;

We engage in minimal data retention and deploy advanced anonymization technologies to protect personal information and sensitive data from being exposed or improperly used.

Read our privacy policy here - [https://portkey.ai/privacy](https://portkey.ai/privacy)[-policy](https://portkey.ai/privacy-policy)

## **System Integrity and Reliability**

### **Network and System Security**:&#x20;

We protect our systems with advanced firewall technologies and DDoS prevention mechanisms to thwart a wide range of online threats. Our security measures are designed to shield our infrastructure from malicious attacks and ensure continuous service availability.

### **Reliability and Availability**:&#x20;

Portkey AI offers an industry-leading [99.995% uptime](https://status.portkey.ai), supported by a global network of 310 data centers.&#x20;

This extensive distribution allows for effective load balancing and edge deployments, minimizing latency and ensuring fast, reliable service delivery across geographical locations.&#x20;

Our failover mechanisms are sophisticated, designed to handle unexpected scenarios seamlessly and without service interruption.

## **Incident Management and Continuous Improvement**

### **Incident Response**

Our proactive incident response team is equipped with the tools and procedures necessary to quickly address and resolve security incidents.&#x20;

This includes comprehensive risk assessments, immediate containment actions, and detailed investigations to prevent future occurrences.&#x20;

We maintain transparent communication with our clients throughout the incident management process. Please review our [status page](https://status.portkey.ai) for incident reports.

### **Updates and Continuous Improvement**

Security at Portkey AI is dynamic; we continually refine our security measures and systems to address emerging threats and incorporate best practices. Our ongoing commitment to improvement helps us stay ahead of the curve in cybersecurity and operational performance.

## **Contact Information**

For more detailed information or specific inquiries regarding our security measures, please reach out to our support team:

* **Email**: support@portkeyai.com, dpo@portkey.ai

## Useful Links

[Privacy Policy](https://portkey.ai/privacy-policy)

[Terms of Service](https://portkey.ai/terms)

[Data Processing Agreement](https://portkey.ai/dpa)

[Trust Portal](https://trust.portkey.ai)
