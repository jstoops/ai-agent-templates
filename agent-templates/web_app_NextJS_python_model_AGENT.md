## Business Requirements

This project is building a [project name] App. Key features:
- Anyone can register an account and sign in; users can change their password or delete their account
- A signed-in user...
- There is an AI chat feature in a sidebar; the AI is able to ...

## Limitations

A demo account (`user` / `password`) is created at startup.

Boards are private to their owner; there is no sharing between users yet.

The app runs locally (in a docker container)

## Technical Decisions

- NextJS frontend
- Python FastAPI backend, including serving the static NextJS site at /
- Everything packaged into a Docker container
- Use "uv" as the package manager for python in the Docker container
- Use OpenRouter for the AI calls. An OPENROUTER_API_KEY is in .env in the project root
- Use `openai/gpt-oss-120b` as the model
- Use SQLLite local database for the database, creating a new db if it doesn't exist
- Start and Stop server scripts for Mac, PC, Linux in scripts/