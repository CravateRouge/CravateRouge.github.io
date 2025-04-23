---
title: 'Phishing Simulation'
type: landing

design:
  spacing: "0rem"

sections:
  - block: hero-rel
    content:
      title: Phishing Simulation
      text: Did you know that 91% of successful data breaches started with a spear phishing attack?
      secondary_action:
        text: What is a Phishing attack?
        url: "#definition"
      announcement:
        text: "Discover how do we"
        link:
          text: "phish"
          url: "#process"
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

  - block: cta-image-paragraph-custom
    id: definition
    content:
      items:
        - title: Phishing...what's that? New technique to catch a fish?
          image: phish.svg
          text: |
            Phishing is the process of attempting to acquire sensitive information such as usernames, passwords and credit card details by masquerading as a trustworthy entity using bulk email which tries to evade spam filters.

            Emails claiming to be from popular social websites, banks, auction sites, or IT administrators are commonly used to lure the unsuspecting public. It’s a form of criminally fraudulent social engineering.

  - block: features
    id: types
    content:
      title: The most common types of phishing
      text: "Of the hundreds of the known phishing scams that exist, here are the four most common types:"
      items:
        - name: E-mail
          icon: envelope
          description: |
            In an email phishing attack, a sense of urgency is predominant. Scammers send out legitimate-looking emails to multiple recipients, encouraging them to modify their passwords or update personal information and account details.
        - name: Smishing
          icon: chat-bubble-bottom-center-text
          description: |
            This phishing tactic closely resembles phishing emails. Hackers try to steal confidential information from individuals by sending text messages (SMS) insisting they respond or take further action. If the individual refuses to do so, the criminals sometimes go as far as threatening them.
        - name: Spear Phishing
          icon: finger-print
          description: |
            This tactic requires the use of emails to conduct an attack against a particular individual or business. The criminal acquires personal information about their target and uses it to send them a personalized and trustworthy email.
        - name: CEO Fraud
          icon: identification
          description: |
            Cyber criminals send emails pretending to be a C-level executive or simply a colleague, usually requesting a fund transfer or tax information.
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
  
  - block: cta-image-paragraph-custom
    parity: "odd"
    content:
      items:
        - title: What's a Phishing Simulation and why it's important?
          image: laptop-phish.png
          text: |
            Phishing simulations are imitations of real-world phishing emails organizations can send to employees to test online behavior and assess knowledge levels regarding phishing attacks. The emails mirror cyber threats professionals may encounter in their daily activities, both during and outside work hours.
            Recent statistics show phishing threats continue to rise. Since 2019, the number of phishing attacks has grown by 150% percent per year—with the Anti-Phishing Working Group (APWG) reporting an all-time high for phishing in 2022, logging more than 4.7 million phishing sites. According to Proofpoint, 84% of organizations in 2022 experienced at least one successful phishing attack.

            Because even the best email gateways and security tools can’t protect organizations from every phishing campaign, organizations increasingly turn to phishing simulations. Well-crafted phishing simulations help mitigate the impact of phishing attacks in two important ways. Simulations provide information security teams need to educate employees to better recognize and avoid real-life phishing attacks. They also help security teams pinpoint vulnerabilites, improve overall incident response and reduce the risk of data breaches and financial losses from successful phishing attempts.

  - block: defile
    id: process
    design:
      css_class: "bg-gray-100 dark:bg-gray-900"
    content:
      title: Our phishing simulation process
      text: "The process generally involves five steps:"
      items:
        - name: Planning
          icon: calendar-days
          description: |
            We first define the objectives and set the scope, deciding which type of phishing emails to use and the frequency of simulations. We'll also determine the target audience, including segmenting specific groups or departments.
        - name: Drafting
          icon: document-text
          description: |
            After forming a plan, we'll create realistic mock phishing emails that closely resemble real phishing threats, modeled on phishing templates and phishing kits available on the dark web. We pay close attention to details like subject lines, sender addresses and content to make realistic phishing simulations. We'll also include social engineering tactics—even impersonating (or ‘spoofing’) an executive or fellow employee as the sender—to increase the likelihood that employees click the emails.
        - name: Sending
          icon: paper-airplane
          description: |
            Once we finalize the content, we will send the simulated phishing emails to the target audience through secure means, with privacy in mind.
        - name: Monitoring
          icon: eye
          description: |
            After sending the mock malicious emails, we'll closely track and record how employees interact with the simulated emails, monitoring if they click on links, download attachments or provide sensitive information.
        - name: Analyzing
          icon: magnifying-glass
          description: |
            Following the phishing test, we will analyze the data from the simulation to determine trends like click rates and security vulnerabilities. Afterward, we will follow up with employees who failed the simulation with immediate feedback, explaining how they could’ve properly identified the phishing attempt and how to avoid real attacks in the future.

  - block: cta-card
    content: 
      title: Education is the best way to prevent phishing attack
      text: |
        Implementing phishing simulation training is the ideal recipe for strengthening data protection. It will help keep your employees alert and aware of all phishing-related scams they may encounter.
      button:
        text: Order your simulation
        url: /contact/
    design:
      card:
        css_class: "bg-primary-700"
      spacing:
        padding: ["1rem", 0, 0, 0]
---