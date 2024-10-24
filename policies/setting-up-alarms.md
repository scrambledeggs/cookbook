# AWS CloudWatch Alarm Setup Policy for Booky Services

### Purpose
This policy outlines the steps for configuring AWS CloudWatch Alarms to monitor booky services, specifically focusing on AWS Lambda functions and AWS X-Ray for distributed tracing. The goal is to ensure that performance, availability, and error conditions are effectively tracked, and appropriate alerts are triggered when issues arise.

### Scope
This applies to all developers, DevOps engineers, and technical personnel responsible for managing and monitoring the booky services deployed on AWS Lambda.

---

## 1. Metrics to Monitor

We’ll monitor key Lambda and X-Ray metrics to ensure everything is running smoothly:

1. **Lambda Function Error Rate**
   If Lambda functions fail, we need to be alerted.
   - **Metric**: `AWS/Lambda - Errors`
   - **Threshold**: Trigger an alarm if the error rate exceeds **1%** for 5 minutes.

2. **X-Ray Faults**
   Monitor X-Ray for fault rates to catch errors in distributed services.
   - **Metric**: `AWS/XRay - FaultCount`
   - **Threshold**: Trigger an alarm if **more than 30% fault rate** are detected.

3. **Invocations**
   Keep an eye on the number of Lambda invocations to monitor service activity.
   - **Metric**: `AWS/Lambda - Invocations`
   - **Threshold**: Set a threshold based on the expected normal volume of invocations.

---

## 2. Alarms and Notifications

### CloudWatch Alarms
For each metric, configure alarms with the following settings:

- **Evaluation Period**: Minimum of **5 minutes**.
- **Datapoints to Alarm**: Use **2 out of 3** evaluation periods to reduce false positives.
- **Actions**: Send alerts and, where possible, automate responses.

### Notification Channels
Set up notification channels to ensure alerts reach the right people:

1. **Slack**: Use Slack **cloudwatch-alarm-notification** channel for critical alerts, such as when Lambda functions fail or X-Ray faults spike.

---

## 3. Alarm Severity Levels

We categorize alarms by their severity to prioritize responses:

- **Critical**: Immediate attention required (e.g., Lambda function error rate exceeds 1%).
- **Warning**: Potential issue that needs monitoring (e.g., increased Lambda duration).
- **Info**: Non-urgent alerts for tracking purposes (e.g., slight latency increases in X-Ray).

---

## 4. Testing and Review

1. **Test Alarms**: Regularly simulate conditions to test the alarms and ensure they’re working correctly.
2. **Review Thresholds**: Periodically revisit and adjust thresholds to match current service usage patterns and performance expectations.

---

## 5. Dashboards and Reports

1. **CloudWatch Dashboard**: Build a real-time dashboard to monitor Lambda and X-Ray metrics.
2. **Weekly Reports**: Generate weekly reports summarizing Lambda invocations, errors, and X-Ray traces for trend analysis.

---

## 6. Policy Compliance

- All alarms and thresholds must be documented and reviewed by the DevOps team.
- Any changes to alarm settings must be approved and updated in the documentation.

---

This policy ensures that booky services using AWS Lambda and X-Ray are continuously monitored, allowing us to address issues before they affect the user experience.
