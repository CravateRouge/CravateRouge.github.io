---
title: 'Audit'
type: landing

design:
  spacing: "0rem"

sections:
  - block: hero-rel
    content:
      title: Penetration Testing Services
      text: Discover the strength of your security controls with infrastructure protection services from trusted cybersecurity experts
      secondary_action:
        text: Understand the methodology
        url: "#methodology"
      announcement:
        text: "Discover our pentest"
        link:
          text: "services"
          url: "#services"
    design:
      css_class: "dark"
      background:
        color: "navy"
        image:
          # Add your image background to `assets/media/`.
          filename: constellation.svg
          size: "auto;background-repeat:repeat"
          filters:
            brightness: 0.5

  - block: features
    id: services
    content:
      title: What scope for your pentest?
      text: Test all or part of your organization
      items:
        - name: External
          icon: share
          description: |
            Test the security strength of all or part of your exposed information system, which could be exploited by hackers or malware.
        - name: Internal
          icon: building-office-2
          description: |
            Test the security strength of your company against an internal attacker with certain access (VPN access, phishing compromise, Wi-Fi access, etc.). Perfect for testing the security of your Active Directory, Network, and Wi-Fi.
        - name: Web/Application
          icon: globe-alt
          description: |
            Assess the security of your applications and websites by detecting vulnerabilities established by OWASP.
        - name: Cloud
          icon: cloud
          description: |
            Assess the security of your Private Cloud or Public Cloud provider (Azure, O365, AWS, GCP).
        - name: Red Team
          icon: user
          description: |
            Simulate a realistic attack to test your organization's resilience against advanced intrusion scenarios.
        - name: Purple Team
          icon: users
          description: |
            Combine the strengths of attack and defense by collaborating with the SOC (Security Operations Center) to improve your threat detection and response.
        - name: Mobile
          icon: device-phone-mobile
          description: |
            Assess the security of your Android and iOS applications by identifying vulnerabilities and protecting sensitive data.
        - name: AV/EDR
          icon: eye
          description: |
            Analyze the coverage and configuration of the EDR/Antivirus deployed on your network to improve its effectiveness in detecting attacks.
        - name: Physical Intrusion
          icon: wrench-screwdriver
          description: |
            Discover weaknesses in the physical security of your company's premises, such as unlocked doors, insufficient surveillance systems, or inappropriate access procedures.
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"

  - block: cta-image-paragraph-custom
    id: methodology
    design:
      css_class_primary: "bg-gray-100 dark:bg-gray-900"
    content:
      title: The methodology of a pentest
      text: Choose the approach that fits your needs and constraints.
      items:
        - title: Black Box
          image: black-box.svg
          text: |
            The black box test involves performing a pentest without any prior knowledge of the target environment. This replicates a realistic external attack, primarily testing the attack surfaces exposed to the public, such as web applications or open systems. This approach measures the company's real ability to withstand an external attacker without initial privileges. However, it may lack depth in identifying complex internal vulnerabilities, which is compensated by white box or grey box methods that explore internal vulnerabilities with more precision.
        - title: White Box
          image: white-box.svg
          text: |
            A white box audit involves conducting a penetration test with access to all necessary information about the target infrastructure: source code, network architecture, system configurations, etc. This method quickly identifies deep and complex vulnerabilities that would be difficult to detect otherwise. It offers comprehensive test coverage, optimizing resources and time. However, this approach may lack realism in cases where an external attacker would not have such information, which is where grey box or black box testing can complement by simulating more realistic external attacks.
        - title: Grey Box
          image: grey-box.svg
          text: |
            The grey box test requires partial information about your system, such as limited credentials or restricted access. This method simulates an attack carried out by a malicious user with partial access to your infrastructure. It evaluates internal security while reproducing realistic scenarios. It is a compromise between black box and white box approaches for balanced analysis.

  - block: cta-card
    content: 
      title: Find the Right Security and Pentesting Services for Your Needs
      text: |
        As cybersecurity breaches continue to threaten the stability of companies in every industry, it’s important to find the right vendor to perform a valid and beneficial assessment of your security. We understand every organization has unique security objectives and challenges, and we strive to tailor our services to meet your needs.
      button:
        text: Request a quote
        url: /contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---