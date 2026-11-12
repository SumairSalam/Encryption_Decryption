# RSA Key and X.509 Certificate Generator

A Windows desktop application built with **C#**, **Windows Forms**, and **BouncyCastle** for generating RSA key pairs and X.509 certificates.

Although the repository is named `Encryption_Decryption`, the current implementation focuses on certificate and key generation.

## Features

- Generate a 2048-bit RSA key pair.
- Create an X.509 v3 certificate.
- Configure certificate subject and issuer names.
- Set certificate validity in months.
- Sign the certificate using SHA-256 with RSA.
- Export the certificate in DER format.
- Export public and private keys in PEM format.
- Select an output folder through a desktop interface.

## Technologies

| Technology | Purpose |
|------------|---------|
| C# | Application logic |
| Windows Forms | Desktop interface |
| BouncyCastle 1.8.9 | RSA key generation and certificate creation |
| .NET | Application runtime |

## How It Works

1. The user enters the certificate subject and issuer.
2. The user selects the validity period and output folder.
3. The application generates an RSA key pair.
4. It builds an X.509 certificate containing the public key.
5. It signs the certificate using the generated private key.
6. It exports the certificate and key files.

The certificate is signed with its own generated key rather than a separate certificate authority's key. Entering an issuer name does not establish trust or create a CA-issued certificate.

## Generated Files

| Filename | Contents |
|----------|----------|
| `X509Cert.der` | X.509 certificate encoded in DER format |
| `X509Cert-public.pem` | RSA public key in PEM format |
| `X509Cert-private.pem` | RSA private key in PEM format |

The public PEM file contains a public key, not a PEM-encoded certificate.

**Important:** The private key is exported without password protection. Keep it private and do not commit generated private keys to Git.

Generating files again in the same folder overwrites the existing files with these names.

## Repository Contents

| File | Description |
|------|-------------|
| `Cryptography.cs` | RSA key generation and X.509 certificate creation |
| `Form1.cs` | Form event handlers and file export logic |
| `Form1.Designer.cs` | Windows Forms interface layout |
| `Form1.resx` | Form resources |
| `Program.cs` | Application entry point |
| `GenCertificate.csproj` | .NET 5 Windows Forms project with BouncyCastle |
| `Encryption.csproj` | Alternate project targeting .NET Core 3.1 |

## Requirements

- Windows
- Visual Studio with the **.NET desktop development** workload, or a compatible .NET SDK
- NuGet package restore access

The project files target older .NET versions. Updating to a supported Windows-targeted framework is recommended before continued development, with compatibility testing.

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/SumairSalam/Encryption_Decryption.git
cd Encryption_Decryption
```

### 2. Open the Project

Open `GenCertificate.csproj` in Visual Studio.

Use this project because it includes the BouncyCastle dependency required by the source code.

### 3. Restore and Build

Restore NuGet packages, then build the project.

With a compatible SDK installed, you can also use:

```bash
dotnet restore GenCertificate.csproj
dotnet build GenCertificate.csproj
dotnet run --project GenCertificate.csproj
```

These commands have not been independently tested.

### Compatibility Notes

- `GenCertificate.csproj` targets `net5.0-windows`.
- `Encryption.csproj` targets `netcoreapp3.1` and does not declare the BouncyCastle dependency.
- The source uses C# target-typed `new()` expressions, so the older alternate project may require language-version or source updates.
- Explicitly specify the project when using the CLI because the repository contains two project files.

## Usage

1. Launch the application.
2. Enter an issuer distinguished name, for example:

   ```text
   CN=Example Certificate Issuer
   ```

3. Enter a subject distinguished name, for example:

   ```text
   CN=example.local, O=Example Organization, C=PK
   ```

4. Select the validity period in months.
5. Click **Browse...** and choose an output folder.
6. Click **Generate**.
7. Find the certificate and key files in the selected folder.

For a conventional self-signed certificate, use matching subject and issuer names.

## Certificate Configuration

| Setting | Implementation |
|---------|----------------|
| Certificate version | X.509 v3 |
| RSA key size | 2048 bits by default |
| Signature algorithm | SHA256WithRSA |
| Serial number | Random positive integer |
| Validity start | Current UTC date |
| Validity end | Start date plus selected months |
| Key usage extension | Key encipherment |

The key size is configurable through the `CreateCertificate()` method but is not exposed in the interface.

## Current Scope and Limitations

- No text or file encryption/decryption workflow is implemented.
- Private keys are exported without encryption.
- The application does not create a trusted CA certificate chain.
- Subject Alternative Names and extended key usages are not configured.
- Certificate installation and trust-store management are not included.
- The repository contains no automated test suite.

## Possible Improvements

- Add encrypted private-key export.
- Add input validation and overwrite confirmation.
- Allow RSA key size selection in the interface.
- Support PEM certificate and password-protected PFX export.
- Add Subject Alternative Name configuration.
- Update the target framework and dependencies.
- Add tests for certificate properties and exported key pairs.

## Attribution and Licensing

Source files contain copyright notices identifying **Everything Embedded (2021)** and restrictions on copying.

No open-source license is included in the repository. Verify ownership and permission before modifying, redistributing, or relicensing the source.

## Repository Maintainer

[Sumair Salam](https://github.com/SumairSalam)
