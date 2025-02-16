# CodeVault---Secure-Password-Manager
#CodeVault is a simple yet powerful password manager built with Node.js. It helps you securely store and retrieve passwords with encryption, ensuring your credentials stay protected.

#javascript 

// CodeVault - Simple Password Manager

const fs = require('fs');
const crypto = require('crypto');

const file = "passwords.json";

// Encrypt function
function encrypt(text) {
    return crypto.createHash('sha256').update(text).digest('hex');
}

// Add a new password
function addPassword(website, username, password) {
    let data = {};
    if (fs.existsSync(file)) {
        data = JSON.parse(fs.readFileSync(file));
    }
    data[website] = { username, password: encrypt(password) };
    fs.writeFileSync(file, JSON.stringify(data, null, 2));
    console.log(`✅ Password saved for ${website}`);
}

// Retrieve password
function getPassword(website) {
    if (!fs.existsSync(file)) {
        console.log("⚠ No passwords saved yet.");
        return;
    }
    const data = JSON.parse(fs.readFileSync(file));
    if (data[website]) {
        console.log(`🔑 ${website} - Username: ${data[website].username}, Password: ${data[website].password}`);
    } else {
        console.log("❌ No password found for this website.");
    }
}

// Example Usage
addPassword("github.com", "user123", "mypassword123");
getPassword("github.com");
