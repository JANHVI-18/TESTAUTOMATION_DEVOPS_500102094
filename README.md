Test Automation with FastAPI and Requests 🚀
In this tutorial, we will explore how to automate backend API testing using FastAPI for creating the server and Requests for testing. By following this structured approach, you'll be able to set up, test, and expand your API testing for real-world applications!

1. Setting Up the FastAPI Server 🛠️
Install FastAPI and Uvicorn
Before we begin, make sure you have FastAPI and Uvicorn installed:

bash
Copy
Edit
pip install fastapi uvicorn
apiserver.py 🖥️
python
Copy
Edit
from fastapi import FastAPI

# Initialize the FastAPI app
app = FastAPI()

# Root endpoint
@app.get("/")
def read_root():
    return {"Hello": "World"}

# Addition endpoint
@app.get("/add/{num1}/{num2}")
def add(num1: int, num2: int):
    """
    Adds two numbers.
    Example: /add/2/3 returns {"result": 5}
    """
    return {"result": num1 + num2}

# Subtraction endpoint
@app.get("/subtract/{num1}/{num2}")
def subtract(num1: int, num2: int):
    """
    Subtracts the second number from the first.
    Example: /subtract/5/3 returns {"result": 2}
    """
    return {"result": num1 - num2}

# Multiplication endpoint
@app.get("/multiply/{num1}/{num2}")
def multiply(num1: int, num2: int):
    """
    Multiplies two numbers.
    Example: /multiply/2/3 returns {"result": 6}
    """
    return {"result": num1 * num2}

# Run the app using Uvicorn
if __name__ == "__main__":
    import uvicorn
    uvicorn.run("apiserver:app", host="0.0.0.0", port=8000, reload=True)
2. Writing Automated Tests using Requests 🧪
Once the API server is up and running, we can write automated tests to verify that the API behaves as expected.

testAutomation.py 🔍
python
Copy
Edit
import requests

# Define test cases for the API endpoints
testcases = [
    {
        "url": "http://localhost:8000/add/2/2",
        "expected": 4,
        "description": "Test addition of 2 and 2"
    },
    {
        "url": "http://localhost:8000/subtract/2/2",
        "expected": 0,
        "description": "Test subtraction of 2 from 2"
    },
    {
        "url": "http://localhost:8000/multiply/2/2",
        "expected": 4,
        "description": "Test multiplication of 2 and 2"
    }
]

def test():
    """
    Runs automated tests on the API endpoints.
    Asserts that the API response matches the expected result.
    """
    for case in testcases:
        # Make a GET request to the API endpoint
        response = requests.get(case["url"])
        
        # Parse the JSON response
        result = response.json()["result"]
        
        # Assert that the result matches the expected value
        assert result == case["expected"], f"Test failed: {case['description']}. Expected {case['expected']}, got {result}"
        
        # Print success message if the test passes
        print(f"Test passed: {case['description']}")
    
    print("All tests passed!")

# Run the test function
test()
3. Enhancing Tests with Pytest 🔧
Using pytest can greatly improve our test automation by providing additional features like parameterized testing, assertions, and better reporting.

Expanded Example with Pytest:
python
Copy
Edit
import pytest
import requests

# Define test cases as a list of tuples for parameterized testing
testcases = [
    ("http://localhost:8000/add/2/2", 4, "Test addition of 2 and 2"),
    ("http://localhost:8000/subtract/2/2", 0, "Test subtraction of 2 from 2"),
    ("http://localhost:8000/multiply/2/2", 4, "Test multiplication of 2 and 2"),
    ("http://localhost:8000/add/-1/1", 0, "Test addition of -1 and 1"),
    ("http://localhost:8000/multiply/0/5", 0, "Test multiplication by zero"),
]

@pytest.mark.parametrize("url, expected, description", testcases)
def test_api(url, expected, description):
    """
    Parameterized test for API endpoints.
    """
    response = requests.get(url)
    result = response.json()["result"]
    print(f"{description}. Expected {expected}, got {result}")
    assert result == expected, f"{description}. Expected {expected}, got {result}"

# Run tests using pytest
if __name__ == "__main__":
    pytest.main()
Key Improvements and Comments:
Descriptive Comments:

Each function and endpoint is clearly documented, explaining its purpose and example usage.

Test Case Descriptions:

Added descriptions for each test case to make debugging easier.

Error Handling:

The assert statement now includes a message with expected and actual results, which helps in identifying failures.

Modularity:

The test framework is modular, making it easy to add or modify tests.

4. Expanding the Idea for Real-World Projects 🌎
1. Add Database Integration 🗃️
Imagine you have an API that interacts with a PostgreSQL or MongoDB database. Testing would then involve:

Checking data integrity after each API call.

Mocking database connections for isolated testing.

2. Authentication and Authorization 🔒
For secure endpoints, you can implement OAuth2, JWT tokens, or API keys for authentication. Tests should validate:

Unauthorized access is properly blocked.

Secure tokens are used to grant access.

3. CI/CD Integration ⚙️
To automate tests on every code change, integrate the tests into a CI/CD pipeline using GitHub Actions, Jenkins, etc. Example GitHub Action:

yaml
Copy
Edit
name: API Tests
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: pip install fastapi uvicorn pytest requests

      - name: Start FastAPI server
        run: python3 apiserver.py &
        env:
          PYTHONUNBUFFERED: 1

      - name: Wait for server to be ready
        run: sleep 5

      - name: Run tests
        run: pytest testAutomation.py
4. Advanced Error Handling & Logging 📜
Use Python's logging module to track API requests and responses, and store logs in CloudWatch or Elastic Stack for better monitoring.

5. Performance & Load Testing 🏋️‍♂️
Simulate high-traffic conditions using tools like Locust or pytest-benchmark for performance testing.

Final Thoughts ✨
In this tutorial, we've:

Built a FastAPI server with basic arithmetic operations.

Automated tests using Requests.

Enhanced the testing process using pytest.

Explored how to scale and integrate testing for real-world projects.

By expanding to features like database integration, authentication, CI/CD pipelines, and load testing, you can take this basic foundation and turn it into a robust solution for any project! Happy coding! 🎉
