# TOTLP.Net

A stateless, cross-platform, and time-synchronized password generator designed for dynamically rotating Linux passwords. Built on `.NET Standard 2.0` to support both modern `.NET Core / .NET 5+` and legacy `.NET Framework` applications.

[![NuGet version](https://img.shields.io/nuget/v/Totlp.svg)](https://www.nuget.org/packages/Totlp/)
[![NuGet Downloads](https://img.shields.io/nuget/dt/Totlp?logo=nuget)](https://www.nuget.org/packages/Totlp/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## Features

- **Stateless & Decentralized:** No database, centralized API, or state management required. Passwords are calculated dynamically using time and a master key.
- **Cross-Platform Consistency:** Uses a custom deterministic Linear Congruential Generator (LCG) to ensure that the exact same password is produced regardless of the OS (Linux/Windows) or .NET runtime version.
- **Cryptographically Secure:** Uses `HMAC-SHA512` with dynamic truncation for core token derivation.
- **Full ASCII Passwords:** Generates complex passwords using the entire printable ASCII range (from `!` to `~`), making them practically immune to brute-force attacks.

---

## Installation

Install the package via NuGet Package Manager Console:

```bash
Install-Package Totlp

```

Or via the .NET CLI:

```bash
dotnet add package Totlp

```

---

## Quick Start

### 1. Generate a Master Token

The master token acts as a shared secret between your application and the target server/infrastructure. You only need to generate this once per target.

```csharp
using System;
using TOTLP.Net;

// Generates a cryptographically secure 16-character Base32 token
string masterKey = Totlp.GenerateToken(16);
Console.WriteLine($"Master Key: {masterKey}"); 
// Example Output: JBSWY3DPEHPK3PXP

```

### 2. Generate the Active Password

Generate the password that is valid for the current hour. Both the client and the server can independently compute this without talking to each other.

```csharp
using System;
using TOTLP.Net;

// Computes a 24-character complex password for the current hour
string currentPassword = Totlp.GenerateCode("JBSWY3DPEHPK3PXP", 24);
Console.WriteLine($"Active Password: {currentPassword}");
// Example Output: dl)=Q`qOnT4htcDJC{EaM_

```

---

## Technical Details

* **Time Step:** Default rotation interval is **1 hour** (3600 seconds).
* **Alphabet:** Standard RFC 4648 Base32 alphabet is used for the master token generation.
* **Password Character Set:** Printable ASCII range `33` to `126`.

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/MrYusuf61/Totlp/blob/main/LICENSE) file for details.


## Security Disclaimer

**IMPORTANT:** This software is provided "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed. 

Since this is a security-related tool (password generation), you are highly encouraged to thoroughly review the source code and adapt it to your environment's specific security policies before running it in a production environment. Under no circumstances shall the author(s) or copyright holders be liable for any direct, indirect, incidental, special, exemplary, or consequential damages (including, but not limited to, procurement of substitute goods or services; loss of use, data, or profits; or business interruption) however caused and on any theory of liability, whether in contract, strict liability, or tort (including negligence or otherwise) arising in any way out of the use of this software, even if advised of the possibility of such damage.
