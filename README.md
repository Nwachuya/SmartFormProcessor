# Smart Form Processor Library

### Overview
A Google Apps Script library that automatically processes Google Forms responses with categorization, keyword extraction, and summaries.

### Installation
1. In your Apps Script project of your form responses.
2. Click **"Libraries"**
3. Enter Library ID: `1pvRGM-KhqH3qJG3Dxt7o8RTp2j6yXW_dJlvbPrY2nB9L7chG2lrFoQGK`
4. Select the latest version
5. Set identifier to "SmartFormProcessor"
6. Click **"Add"**

### Configuration Options

#### categories
**Purpose**: Define how responses should be categorized based on content
**Format**: Object mapping keywords to category names
**Example**:
```javascript
categories: {
  "urgent": "Urgent Priority",     // If response contains "urgent", assign "Urgent Priority" category
  "help": "Support Request",       // If response contains "help", assign "Support Request" category
  "bug": "Bug Report",             // If response contains "bug", assign "Bug Report" category
  "feature": "Feature Request"     // If response contains "feature", assign "Feature Request" category
}
```

#### keywords
**Purpose**: Extract specific keywords from responses for analysis
**Format**: Array of keywords to search for
**Example**:
```javascript
keywords: ["urgent", "help", "feedback", "bug", "issue", "suggestion", "question"]
```
The library will count occurrences of these keywords and provide statistics.

#### outputSheet
**Purpose**: Create a new sheet tab with processed results
**Format**: String with sheet name (optional). 
**Example**:
```javascript
outputSheet: "Processed_Responses"  // Creates a sheet tab named "Processed_Responses" I recommend creating a new tab in the same sheet and using the name
```
If omitted, no output sheet will be created or updated.

#### triggerEmail
**Purpose**: Send summary email notification after processing
**Format**: Email address string (optional)
**Example**:
```javascript
triggerEmail: "admin@company.com"  // Sends summary to this email address
```
If omitted, no email will be sent.

### Functions Available

#### processFormResponses(formId, options)
**Purpose**: Main function that processes all form responses with full analysis
**Parameters**: 
- `formId`: String - Google Form ID
- `options`: Object - Configuration options
**Returns**: Object with processing results
**Example**:
```javascript
function analyzeForm() {
  const formId = "1FAIpQLSf1234567890abcdef";
  const options = {
    categories: {
      "urgent": "Urgent",
      "help": "Support Request",
      "feedback": "Feedback"
    },
    keywords: ["urgent", "help", "feedback", "bug"],
    outputSheet: "Analysis_Results",
    triggerEmail: "manager@company.com"
  };
  
  const result = SmartFormProcessor.processFormResponses(formId, options);
  console.log("Processed " + result.responsesProcessed + " responses");
  console.log("Categories:", result.summary.categoryDistribution);
}
```

#### getFormSummary(formId)
**Purpose**: Get basic statistics about form responses without full processing
**Parameters**: 
- `formId`: String - Google Form ID
**Returns**: Object with summary statistics
**Example**:
```javascript
function getFormStats() {
  const formId = "1FAIpQLSf1234567890abcdef";
  const summary = SmartFormProcessor.getFormSummary(formId);
  
  console.log("Total responses:", summary.totalResponses);
  console.log("Date range:", summary.dateRange.start, "to", summary.dateRange.end);
  console.log("Average response time:", summary.averageResponseTime, "ms");
}
```

#### setupAutoProcessing(formId, options)
**Purpose**: Set up automatic processing when new form responses are submitted
**Parameters**: 
- `formId`: String - Google Form ID
- `options`: Object - Configuration options for processing
**Returns**: String - Trigger ID for the created trigger
**Example**:
```javascript
function setupAutomaticProcessing() {
  const formId = "1FAIpQLSf1234567890abcdef";
  const options = {
    categories: {
      "urgent": "Urgent",
      "help": "Support"
    },
    outputSheet: "Auto_Processed",
    triggerEmail: "notifications@company.com"
  };
  
  const triggerId = SmartFormProcessor.setupAutoProcessing(formId, options);
  console.log("Auto-processing trigger created with ID:", triggerId);
}
```

#### Advanced Categories Configuration
For more sophisticated categorization, use advanced category rules:
```javascript
const advancedCategories = {
  "Urgent": {
    keywords: ["urgent", "asap", "immediately", "emergency"],
    weight: 2,  // Higher weight = higher priority
    regex: ["critical.*error", "system.*down", "can't.*access"]  // Regular expressions
  },
  "Technical Support": {
    keywords: ["error", "bug", "problem", "not working"],
    weight: 1.5,
    regex: ["can't.*login", "error.*message"]
  },
  "General Feedback": {
    keywords: ["feedback", "suggestion", "idea", "improvement"],
    weight: 1
  }
};
```

### Requirements
- Google Workspace account
- Access to the target form
- Apps Script execution quotas
- For output sheets: Access to the spreadsheet where sheet will be created

### Support
For issues, check the Apps Script execution log for error messages. Common issues include:
- Form access permissions
- Exceeding execution quotas
- Invalid form IDs
- Network connectivity issues for email notifications
