# Contributing

We welcome contributions! This project is built on the assumption that you have read and agree to our [Code of Conduct](CODE_OF_CONDUCT.md).

## Getting Started

1. Fork this repository.
2. Clone your fork: `git clone https://github.com/your-handle/cloud-itonami-isco-2162.git`
3. Create a feature branch: `git checkout -b feature/your-feature`
4. Make your changes and test them: `clojure -M:test`
5. Commit your changes with clear, concise messages.
6. Push to your fork and open a pull request.

## Development

This is a Clojure project using `deps.edn`. Tests run via the Cognitect test runner:

```bash
clojure -M:test
```

All code must be portable (`.cljc` files preferred) and pass tests before submission.

## Scope

Contributions should align with the ISCO-08 2162 landscape architecture domain and the itonami actor pattern. Please open an issue first to discuss major changes.

## License

By contributing, you agree to license your work under [AGPL-3.0-or-later](LICENSE).
