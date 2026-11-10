<p align="center">
  <a href="https://nakodax.com/source">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://nakodax.com/branding/nakodax-lockup.svg">
      <img alt="NakodaX" src="https://nakodax.com/branding/nakodax-lockup-on-light.svg" width="260">
    </picture>
  </a>
</p>

# NakodaX Source Protection: an encrypted codebase, left in public

**Our code is public. Our source isn't.**

![Encryption: AES-256-GCM](https://img.shields.io/badge/encryption-AES--256--GCM-00B26D) ![Protected files: 704](https://img.shields.io/badge/protected%20files-704-00B26D) ![Licence: proprietary](https://img.shields.io/badge/licence-proprietary-555) ![Runtimes: Node.js and Python](https://img.shields.io/badge/runtimes-Node.js%20%7C%20Python-555)

**We protect our own code with NakodaX. This repository is what that looks like from the outside.**

NakodaX Source Protection encrypts source code so customers, contractors and AI tools can hold a repository without being able to read it. Clone this one, open any file, and see what someone without a licence sees.

<p align="center">
  <img src="https://nakodax.com/illustrations/source-hero-4.webp" width="100%" alt="A code editor on a glass screen, with one section of the code held inside a glowing green panel clamped shut with two latches: NakodaX Source Protection keeps source code encrypted.">
</p>

[Product page](https://nakodax.com/source) · [Security at NakodaX](https://nakodax.com/security) · [Request review access](ACCESS.md) · [Unlock a protected app yourself: nkx-sample-app](https://github.com/nakodax/nkx-sample-app) · [Report a security issue](SECURITY.md)

## What is NakodaX Source Protection?

NakodaX Source Protection lets software vendors ship and run their software on customer infrastructure (on-premise, private cloud or air-gapped) without handing over a readable copy of the source code. Each protected file stays encrypted in the repository and on disk. Application code is decrypted in memory by a licensed NakodaX runtime as it loads, and build-time files are opened only inside an approved build. Access is controlled per customer deployment and can be ended at any time.

## This repository at a glance

| | |
|---|---|
| **Product** | NakodaX Source Protection, by NakodaX |
| **What is protected** | 704 files: TypeScript, TSX, Python and SQL source, plus tests, docs, scripts and config |
| **What stays readable** | Folder and file names, this README, `LICENSE`, `ACCESS.md`, `SECURITY.md`, `.gitignore`, and the `.nkx/manifest.json` files |
| **Encryption** | AES-256-GCM, with a separate random key for every file |
| **Key protection** | Each file key is wrapped with a key derived by scrypt (N = 2^17, r = 8, p = 1) and then bound to this build with HKDF-SHA256 |
| **Tamper resistance** | Every ciphertext is bound to its own file path, so renaming or swapping files makes decryption fail |
| **Encrypted files on disk** | `.nkxc`, one per protected file, plus companion stubs used by NakodaX tooling |
| **Runtime SDKs** | [`@nakodax/runtime`](https://www.npmjs.com/package/@nakodax/runtime) for Node.js, [`nkx-runtime`](https://pypi.org/project/nkx-runtime/) for Python |
| **Licence** | Proprietary, public to inspect, not open source. See [`LICENSE`](LICENSE) |
| **Contact** | support@nakodax.com |

## How does NakodaX Source Protection work?

1. **Protect.** NakodaX encrypts each selected file with its own AES-256-GCM key and records it in a manifest. The readable file is removed from the repository.
2. **Ship.** The encrypted repository, or software built from it, goes to the customer, a partner or a contractor. Folder structure and file names stay the same, so builds and deployments keep their shape.
3. **Run.** At start-up, the NakodaX runtime checks the licence, unwraps the file keys and decrypts each module in memory as it loads. The readable code is never written to the customer's disk.
4. **Control.** Access is set per customer deployment. Ending a licence, or rotating the key for a new release, stops that deployment from opening the code, without affecting other customers.

## What can someone see without a key?

- **The layout.** Folder and file names are readable, as in any NakodaX protected repository.
- **Ciphertext.** Every `.nkxc` file is random-looking bytes. There is no code, comment or string to read.
- **The manifest.** `.nkx/manifest.json` lists each protected file with its wrapped key, its hashes and its language. Every file key in it is wrapped, and is useless without a valid NakodaX licence.
- **These documents:** this README, [`LICENSE`](LICENSE), [`ACCESS.md`](ACCESS.md), [`SECURITY.md`](SECURITY.md) and `.gitignore`.

## Check it yourself in one minute

```bash
git clone https://github.com/nakodax/nkx-source-protection.git
cd nkx-source-protection

# Count the encrypted files
git ls-files '*.nkxc' | wc -l

# Look inside one: random bytes, no source code
head -c 96 backend/src/app.ts.nkxc | xxd

# Search every encrypted file for code keywords: no matches
grep -l -a -E '^(import|export|def|class|function) ' -r --include='*.nkxc' . | wc -l
```

**About this snapshot.** This repository is published as a single squashed commit of a protected release (November 2026). Development history and individual commits are not included in public snapshots.

## Who is NakodaX Source Protection for?

- **Software vendors** who deploy into customer environments: on-premise, private cloud, regulated or air-gapped.
- **Teams that share code** with contractors, outsourcing partners or offshore developers, and need it back when the engagement ends.
- **Companies worried about AI.** A readable codebase can be pasted into an AI tool, used for training, or reshaped into a competing product. An encrypted one gives an AI model nothing to work with.
- **Founders and CTOs in due diligence** who need to show code to buyers or investors without handing it over.

## How is this different from other ways to protect source code?

| Approach | Can the recipient read the code? | Can it run? | Can access be ended later? |
|---|---|---|---|
| **NakodaX Source Protection** | No. Files are encrypted at rest | Yes, with a licensed runtime | Yes, per deployment |
| Obfuscation or minification | Yes, with effort | Yes | No |
| Shipping compiled binaries only | Partly, by decompiling | Yes | No |
| Private repository access | Yes | Yes | Only for future access, not copies already taken |
| Contracts and NDAs alone | Yes | Yes | On paper only |

## Frequently asked questions

### What is source code protection?

Source code protection keeps the readable version of software away from people who run or receive it. NakodaX Source Protection does this with encryption: the code stays encrypted on disk and is decrypted only in memory by a licensed runtime.

### Is this repository open source?

No. It is public, not open source. You can clone it and read the five documents and the manifest. You may not decrypt, copy or redistribute the protected files, or use them to train AI models. See [`LICENSE`](LICENSE).

### Can an AI model or LLM read this repository?

No. Without a licence key, the protected files are ciphertext. An AI model, a code search index or a scraper learns nothing from them beyond file names.

### How is encryption different from obfuscation?

Obfuscated code is still code: anyone can run it, and with enough effort read it. An encrypted file cannot be read or run until a licensed NakodaX runtime decrypts it.

### Can I build or run this repository?

Not without a key. To see NakodaX protected software unlock and run on your own machine, use [nkx-sample-app](https://github.com/nakodax/nkx-sample-app), which ships with a public trial licence.

### What happens when a customer's licence ends?

The runtime checks the licence at start-up, so that deployment can no longer open the protected code. Other customers are not affected. For a new release, rotating the key means old licences cannot open it.

### Does NakodaX see or keep my source code?

No. Protection runs in an isolated, single-use container. No one on the NakodaX team has access to it, and it is destroyed as soon as protection finishes. NakodaX does not store or keep a readable copy of your code.

### Which languages does NakodaX Source Protection support?

This repository includes TypeScript, Python and SQL, with docs, scripts and config protected as text. See [nakodax.com/source](https://nakodax.com/source) for the full list.

### How do I get access to review the code?

Email **support@nakodax.com** with the subject `Source review: nkx-source-protection`. Customers, prospective customers, investors, auditors and security researchers with a specific question are reviewed case by case. [`ACCESS.md`](ACCESS.md) has the details.

## Key terms

- **`.nkxc` file:** one file's ciphertext. Useless without the manifest and a valid licence.
- **Manifest (`.nkx/manifest.json`):** the list of protected files, with each file's wrapped key and hashes.
- **Licensed runtime:** the NakodaX loader for Node.js or Python that checks the licence and decrypts code in memory.
- **Key rotation:** issuing a new key for a release, so licences for older releases cannot open it.

## Related

- [nkx-sample-app](https://github.com/nakodax/nkx-sample-app): the same protection, with a public trial key, running on your own machine
- [nkx-cli](https://github.com/nakodax/nkx-cli): the command-line tool for CI builds and approved change sessions
- Runtime SDKs: [`@nakodax/runtime`](https://www.npmjs.com/package/@nakodax/runtime) (Node.js) and [`nkx-runtime`](https://pypi.org/project/nkx-runtime/) (Python)
- [nkx-data-protection](https://github.com/nakodax/nkx-data-protection) and [nkx-document-protection](https://github.com/nakodax/nkx-document-protection): NakodaX Data Pipelines and NakodaX Document Protection, protected the same way
- Website: [nakodax.com](https://nakodax.com) · Platform: [app.nakodax.com](https://app.nakodax.com) · Support: [nakodax.com/support](https://nakodax.com/support)

## Security and compliance

- **CSA STAR Level 1:** [listed on the CSA STAR Registry](https://cloudsecurityalliance.org/star/registry/nakodax-technologies-private-limited/services/nakodax), with our CAIQ answers public.
- **ISO/IEC 5230 and ISO/IEC 18974:** open source licence compliance and open source security assurance, [listed by OpenChain](https://openchainproject.org/community-of-conformance) ([announcement](https://openchainproject.org/news/2026/11/05/nakodax-announces-an-openchain-conformant-program)).
- **NIST CSF 2.0:** alignment across all 22 categories, updated every quarter.
- **NIST SSDF (SP 800-218):** our software development practices follow the Secure Software Development Framework.

Full details are on [nakodax.com/security](https://nakodax.com/security). For security documentation or a questionnaire, email security@nakodax.com.

## Security and licence

Found a problem? See [`SECURITY.md`](SECURITY.md). Proprietary, all rights reserved: [`LICENSE`](LICENSE).

*Last updated: November 2026.*
