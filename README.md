from flask import Flask, request, jsonify
import re

app = Flask(__name__)

# Regex pattern for a strong password
PASSWORD_REGEX = re.compile(
    r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$'
)

def is_strong_password(password):
    return PASSWORD_REGEX.match(password) is not None

@app.route('/signup', methods=['POST'])
def signup():
    data = request.get_json()

    if not data or not data.get('username') or not data.get('password'):
        return jsonify({"error": "Username and password are required"}), 400

    username = data['username']
    password = data['password']

    if not is_strong_password(password):
        return jsonify({"error": "Password is not strong enough"}), 400

    # Here you would normally save the user to your database
    # For this example, we just return a success message

    return jsonify({"message": "User signed up successfully"}), 201

if __name__ == '__main__':
    app.run(debug=True)
