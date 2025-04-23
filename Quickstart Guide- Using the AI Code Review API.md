# Quickstart Guide: Using the AI Code Review API
### Overview
This guide provides a step-by-step walkthrough for integrating the AI Code Review API into your development workflow. You'll learn how to authenticate, submit source code for analysis, check review status, and retrieve structured results — all using standard HTTP requests.
This example uses curl, but the same structure can be applied using Postman, Insomnia, or any HTTP client.
### Prerequisites
Before you begin, ensure the following:
- You have an API key from AI Code Check
- curl is installed on your system (or use a REST client like Postman)
- You have a code snippet in one of the supported languages:
- Rust
- Solidity
- JavaScript
- Python
### Step 1: Authenticate
  Use your API key to obtain an access token.
	\`\`\`bash`
	curl -X POST https://api.aicodecheck.com/v1/auth/token \
	  -H "Content-Type: application/json" \
	  -d '{
	"api_key": "your_api_key_here"
	  }'
Example Response
	\`\`\`json`
	{
	  "access\_token": "abc123xyz456token",
	  "expires\_in": 3600
	}
Save your access token. You’ll need it in the **Authorization** header for all future requests
### Step 2: Submit Code for Review
Submit a code snippet and specify the language for analysis
	\`\`\`bash`
	curl -X POST https://api.aicodecheck.com/v1/review/submit \
	  -H "Authorization: Bearer abc123xyz456token" \
	  -H "Content-Type: application/json" \
	  -d '{
	"language": "rust",
	"source_code": "pub fn close_account(ctx: Context<CloseAccount>) -> Result<()> {\n let token_account = &mut ctx.accounts.token_account;\n token_account.try_borrow_mut_lamports()?;\n Ok(())\n}"
	 ` }'`
Example Response
	\`\`\`json
	{
	  "review\_id": "rev-9012cdef",
	  "status": "pending"
	}
You’ll use this review_id to check the status and fetch results.
Step 3: Check Review Status
Poll the status endpoint until the review is complete.
	\`\`\`bash`
	curl https://api.aicodecheck.com/v1/review/status/rev-9012cdef \
	  -H "Authorization: Bearer abc123xyz456token"
Example Response
	\`\`\`json`
	{
	  "review\_id": "rev-9012cdef",
	  "status": "completed"
	}
### Step 4: Retrieve the Results
Fetch detailed issues and recommendations once the review is complete.
	\`\`\`bash`
	curl https://api.aicodecheck.com/v1/review/results/rev-9012cdef \
	  -H "Authorization: Bearer abc123xyz456token"
Example Response
	\`\`\`json`
	{
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
	}

### Next Steps

Once you've verified the integration:
✅ Automate submission in your CI/CD pipeline
✅ Monitor severity thresholds to prevent deploying risky code
✅ Expand language support or batch submissions