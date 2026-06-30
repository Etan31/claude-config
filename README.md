# CLAUDE.md - Full-Stack Development Guide

A comprehensive coding standards and best practices guide for full-stack web applications built with **React**, **Express**, **Node.js**, and **PostgreSQL/Supabase**.

## What Is This?

This repository contains `CLAUDE.md`, a detailed guide that standardizes code quality, security, documentation, and architecture patterns for development with Claude AI. It covers:

- ✅ Code organization and best practices
- ✅ Backend (Express/Node.js) patterns
- ✅ Frontend (React) efficiency and optimization
- ✅ API security and endpoint standards
- ✅ Database design (PostgreSQL/Supabase)
- ✅ Documentation requirements
- ✅ Testing strategies
- ✅ Deployment and environment setup
- ✅ Version control workflows
- ✅ Data privacy and security restrictions

## Who Is This For?

- Teams building full-stack applications with React + Node.js
- Developers using Supabase or PostgreSQL as their database
- Anyone using Claude AI for code generation and wants consistent patterns
- Projects that need clear coding standards and architecture guidelines

## Quick Start

### 1. Clone This Repository

```bash
git clone https://github.com/yourusername/claude-md.git
cd claude-md
```

### 2. Copy CLAUDE.md to Your Project

```bash
# Copy to your project root
cp CLAUDE.md ../your-project-name/

# Or copy to a docs folder
cp CLAUDE.md ../your-project-name/docs/CLAUDE.md
```

### 3. Customize for Your Project

Edit `CLAUDE.md` to:
- Add project-specific tech stack details
- Adjust performance targets and coverage percentages
- Add links to your internal documentation
- Include your team's naming conventions or deployment process

### 4. Share with Claude AI

When working with Claude AI, include this prompt at the start of your conversation:

```
I'm working on a full-stack project (React + Express + PostgreSQL/Supabase).
Here's my coding standards guide:

[Paste CLAUDE.md content here]

Please follow these guidelines when helping me with code.
```

Or reference it in your `.claudeignore` or project documentation.

## File Structure

```
.
├── CLAUDE.md          # Main coding standards and guidelines
├── README.md          # This file
└── examples/          # (Optional) Example implementations
    ├── api-endpoint.js
    ├── React-component.jsx
    └── database-migration.sql
```

## How to Use CLAUDE.md

### For Claude AI / LLM Prompts

Provide the guide when:
- Starting a new feature or module
- Refactoring existing code
- Setting up API endpoints
- Creating React components
- Designing database schemas

Example prompt:
```
Following CLAUDE.md guidelines, help me create a new API endpoint for user authentication.
- Should use JWT tokens
- Validate input with Zod
- Include proper error handling
```

### For Your Team

- Reference sections in code reviews
- Link to specific sections when discussing patterns
- Use as onboarding material for new developers
- Adapt and version-control your own copy

## Key Sections Overview

| Section | Purpose |
|---------|---------|
| **General Principles** | KISS philosophy, code clarity, documentation habits |
| **Code Quality** | Standards for backend, frontend, and database layers |
| **Documentation** | Comments, README, API docs, schema docs |
| **API Security & Endpoints** | Auth, validation, data protection, rate limiting |
| **Frontend Efficiency** | Performance, state management, rendering optimization |
| **Testing** | Unit and integration test guidelines |
| **Environment & Deployment** | Setup, env variables, production checklist |
| **Version Control** | Commit messages, branching strategy |
| **AI Restrictions** | Data privacy, security, credential handling |

## Customization Examples

### Add Your Company Logo
```markdown
# CLAUDE.md - [Your Company] Development Guide
```

### Adjust Coverage Requirements
Change this line in the Testing section:
```markdown
- Minimum coverage: 70% for critical paths.
```
To your target:
```markdown
- Minimum coverage: 85% for critical paths (measured via CI/CD).
```

### Add Project-Specific Tech
In the Database section, add:
```markdown
### Redis Caching
- Use Redis for session storage and API response caching.
- Key format: `cache:endpoint:userId:params`
- TTL: 5 minutes for user data, 1 hour for static content.
```

### Link to Internal Docs
```markdown
For detailed Supabase RLS setup, see [our RLS guide](https://wiki.company.com/supabase-rls).
```

## Contributing

Found a bug or want to improve the guide?

1. Fork the repository
2. Create a feature branch: `git checkout -b improve/testing-section`
3. Make changes to `CLAUDE.md`
4. Commit with clear messages: `docs: add E2E testing guidelines`
5. Push and create a Pull Request

## Best Practices for Maintenance

- **Version the guide**: Update the date in CLAUDE.md when making changes
- **Document decisions**: Add a CHANGELOG.md if making major updates
- **Collect feedback**: Ask your team if sections need clarification
- **Test in real projects**: Use the guide in actual projects before promoting changes
- **Keep it current**: Review annually and update for new tech/patterns

## Example Projects Using This Guide

Add your project here! (Optional)

- [Project Name](#) - Brief description
- [Another Project](#) - What tech stack

## FAQ

**Q: Can I modify CLAUDE.md for my specific tech stack?**  
A: Yes! This is a template. Adapt it to your needs. Just keep the core sections.

**Q: Should I commit CLAUDE.md to my project repo?**  
A: Yes. Keep a copy in your project root or `/docs` folder for team reference.

**Q: How do I use this with Claude AI / ChatGPT?**  
A: Copy the content and paste it in your prompt, or reference it: "Follow the guidelines in CLAUDE.md"

**Q: What if my project doesn't use Supabase?**  
A: Replace Supabase sections with your database (MongoDB, Firebase, etc.). The principles remain the same.

**Q: Is this a coding standard I can enforce?**  
A: Yes. Use it in code reviews, linting rules, and CI/CD checks. You can even automate some rules with ESLint, Prettier, and pre-commit hooks.

## Resources

- [Express.js Best Practices](https://expressjs.com/en/advanced/best-practice-security.html)
- [React Performance Guide](https://react.dev/reference/react/memo)
- [PostgreSQL Tuning](https://wiki.postgresql.org/wiki/Performance_Optimization)
- [Supabase Security](https://supabase.com/docs/guides/auth)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

You're free to use, modify, and distribute this guide in your projects.

## Contact

- Author: [Your Name](https://github.com/yourusername)
- Questions or suggestions? Open an issue or reach out.

---

**Last Updated:** January 2025  
**Version:** 1.0  
**Maintained By:** [Your Team]
