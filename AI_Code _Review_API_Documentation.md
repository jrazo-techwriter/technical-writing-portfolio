# AI Code Review Service API
Version: 1.0
	Base URL: https://api.aicodecheck.com/v1/
### Introduction
The AI Code Review API allows developers to automate static analysis, vulnerability detection, and best practices enforcement in codebases. It supports popular languages such as Rust, Solidity, JavaScript, and Python.
This document explains how to authenticate, submit code for analysis, check the results, and retrieve detailed reports.
### Authentication
	Endpoint: Post /auth/token
Description:
Obtain an authentication token to use in subsequent API requests. 
Request:
json:
	{
	  "api\_key": "YOUR\_API\_KEY"
	}
Response:
json:
	{
	  "access\_token": "YOUR\_ACCESS\_TOKEN",
	  "expires\_in": 3600
	}
Notes:
- Tokens expire after 1 hour.
	- Include the token in the `Authorization` header as Bearer Your_access_token.
Submit Code for Review
	Endpoint: Post /review/submit
Description:
Upload a source code snippet or file for AI-powered analysis.
Headers:
	- Authorization: Bearer Your_access_token
	- Content-type: application/json
Request:
	{
	  "language": "rust",
	  "source\_code": "pub fn transfer(...) { ... }"
	}
Response:
json:
	{
	  "review\_id": "abc123",
	  "status": "pending"
	}
Notes:
	Supported languages: rust, solidity, JavaScript, python
- Maximum file size: 5MB
- Maximum request size: 10,000 lines of code
### Check Review Status
	Endpoint: GET /review/status/ {review_id}_
Description:
Check the status of your code review request.
Path Parameters:
	review_id – the id returned by the  / submit request._
### Response:
json:
	{
	  "review\_id": "abc123",
	  "status": "completed"
	}
Possibly Status Values:
- Pending 
- Analyzing
- Completed
- Failed
### Retrieve Review Results
Endpoint: `Get /review/results/ {review_id}_`
Description:
Fetch detailed analysis results once the review is completed.
### Response:
json:
	{
	  "review\_id": "abc123",
	  "issues": \[
	{
	  "type": "security",
	  "severity": "high",
	  "message": "Missing access control on critical function",
	  "line": 42,
	  "recommendation": "Add ownership check to the function."
	},
	{
	  "type": "best_practice",
	  "severity": "medium",
	  "message": "Function could be optimized for gas efficiency",
	  "line": 78,
	  "recommendation": "Use `unchecked` block if overflow is impossible."
	}
	  ]
	}
Notes:
- Issues are categorized by type: security, performance, best_practice._
- Severity levels: low, medium, high, critical.
### Error Codes
- 400  Bad Request – Missing parameters in the request
- 401  Unauthorized  – Invalid or expired access token
- 404  Not Found  – Review ID does not exist
- 500  Server Error  – Internal API error
### Rate Limits
- Maximum request: 1000 per day
- Burst limit: 10 requests per second
- Error Code if exceeded: 429 Too Many Requests
Examples
Example: Full Flow
1.  Authenticate and receive a token.
2. Submit Rust code for review.
3. Poll review status.
4. Retrieve issues and recommendations.