


# Backend API Test Automation with FastAPI and Requests

In this tutorial, we'll cover how to automate backend API testing using **FastAPI** for the server and **Requests** for testing. The following steps will be covered:

1. **Setting Up the FastAPI Server**
2. **Running the Server**
3. **Writing Automated Tests using Requests**
4. **Enhancing Tests with Pytest**
5. **Expanding the Idea for Real-World Projects**

---

## 1. Setting Up the FastAPI Server 🚀

We'll first create a simple API with three endpoints: **add, subtract, and multiply**.

### Install FastAPI and Uvicorn

If you haven't already, install FastAPI and Uvicorn:

```bash
pip install fastapi uvicorn
```

### `apiserver.py`

```python
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
```

---

## 2. Running the Server 🔥

To run the server, use the following command:

```bash
uvicorn apiserver:app --reload
```

Your FastAPI server should now be running at `http://localhost:8000`. You can test the endpoints by visiting:

- `http://localhost:8000/add/2/3`
- `http://localhost:8000/subtract/5/3`
- `http://localhost:8000/multiply/2/3`

---

## 3. Writing Automated Tests using Requests 🧪

We will use the **Requests** library to automate the testing of our API endpoints.

### `testAutomation.py`

```python
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
```

---

## 4. Enhancing Tests with Pytest 🔧

For more powerful and readable test automation, we can enhance the tests with **Pytest**.

### `test_automation.py` with Pytest

```python
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
```

Run the tests using **Pytest**:

```bash
pytest test_automation.py
```

---

## 5. Expanding the Idea for Real-World Projects 🌍

### 1. Add Database Integration 💾

Instead of simple arithmetic, imagine an API that fetches/stores data in a **PostgreSQL** or **MongoDB** database. Testing would involve:

- **Checking data integrity** after each API call.
- **Mocking database connections** for isolated testing.

### 2. Authentication and Authorization 🔒

Add **OAuth2, JWT, or API keys** for secure endpoints. Test unauthorized access to ensure security.

### 3. CI/CD Integration 🛠️

Use **GitHub Actions / Jenkins** to automate testing on each code push.

#### Example GitHub Action for Pytest

```yaml
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
        run: python apiserver.py &
        env:
          PYTHONUNBUFFERED: 1

      - name: Wait for server to be ready
        run: sleep 5

      - name: Run tests
        run: pytest test_automation.py
```

### 4. Advanced Error Handling & Logging 📝

Use Python's `logging` module to track API requests and responses. Store logs in **CloudWatch** or **Elastic Stack**.

### 5. Performance & Load Testing 🚅

Use **`pytest-benchmark`** or **`locust.io`** to simulate high-traffic conditions and ensure your API performs well under load.

---

## Final Thoughts 💡

In this tutorial, we:

- Built a **FastAPI server** with three endpoints.
- Wrote **automated tests** using `requests`.
- Improved testing with **pytest**.
- Discussed **expanding to real-world applications**.

