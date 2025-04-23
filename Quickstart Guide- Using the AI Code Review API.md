# Quickstart Guide: Using the AI Code Review API
## Overview
This guide walks you through integrating the AI Code Review API into your development workflow. You’ll learn how to authenticate, submit a code snippet for review, and retrieve analysis results using basic HTTP requests.
This example uses `curl, but the same structure applies to any HTTP client (e.g., Postman, Python  requests, JavaScript  fetch
`### Prerequisites 
Before you begin, ensure you have the following:
- An active API key from AI Code Check
- `curl, installed (or Postman/Insomnia/etc.)
	`- A code snippet to test (Rust, Solidity, JavaScript, Python supported)

### Step 1: Authenticate
Use your API key to generate an across token.
	'''curl -X POST https://api.aicodecheck.com/v1/auth/token \
	  -H "Content-Type: application/json" \
	  -d '{
	"api_key": "your_api_key_here"
	  }'''
Example Response
	json
	{
	“access_token”: “abc123xyz456token”,_
	“expires_in”: 3600_
	}
Save your token – you’ll need it in the `Authorization header for all requests.
`Step 2: Submit Code for Review 
Once you have a token, submit your source code along with the language identifier.
	bash
	'''`curl -X POST https://api.aicodecheck.com/v1/review/submit \`
	  -H "Authorization: Bearer abc123xyz456token" \`
	`  -H "Content-Type: application/json" \`
	  -d '{`
	"language": "rust",
	"source_code": "pub fn close_account(ctx: Context<CloseAccount>) -> Result<()> {\n let token_account = &mut ctx.accounts.token_account;\n token_account.try_borrow_mut_lamports()?;\n Ok(())\n}"
	  }''''
Example Response
	json
	'''{
	  "review\_id": "rev-9012cdef",
	  "status": "pending"
	}'''
Step 3: Check Review Status
Poll the status endpoint until the review is complete.
	bash
	'''curl https://api.aicodecheck.com/v1/review/status/rev-9012cdef \
	  -H "Authorization: Bearer abc123xyz456token"'''
Example Response
	json
	'''{
	  "review\_id": "rev-9012cdef",
	  "status": "completed"
	}'''

Step 4: Retrieve the Results
Once the review is complete, retrieve the list of identified issues and recommendations.
	bash
	'''curl https://api.aicodecheck.com/v1/review/results/rev-9012cdef \
	  -H "Authorization: Bearer abc123xyz456token"'''
Example Response
	json
	'''{
	  "review\_id": "rev-9012cdef",
	  "issues": \[
	{
	  "type": "security",
	  "severity": "high",
	  "line": 2,
	  "message": "Missing signer validation",
	  "recommendation": "Ensure `ctx.accounts.token_account` is signed by an authorized wallet."
	},
	{
	  "type": "best_practice",
	  "severity": "medium",
	  "line": 3,
	  "message": "Unnecessary borrow used",
	  "recommendation": "Use `lamports()` instead of `try_borrow_mut_lamports()` if no mutation is required."
	}
	  ]
	}'''
### Next Steps
Now that your integration is working:
- Automate code submission with CI tools (e.g., GitHub Actions, GitLab CI)
- Set severity thresholds to flag high-risk code before deployment
- Review full API reference for advanced options

