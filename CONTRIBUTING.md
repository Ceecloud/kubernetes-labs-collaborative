# Contributing to Kubernetes Labs

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## 🎯 Ways to Contribute

### 1. Complete Labs & Share Solutions
- Work through the labs
- Share your YAML solutions
- Document your approach
- Help others understand concepts

### 2. Improve Documentation
- Fix typos or unclear instructions
- Add clarifications
- Improve command examples
- Add troubleshooting tips

### 3. Create New Content
- Add optional challenges
- Create advanced variations
- Write troubleshooting guides
- Add diagrams or visuals

### 4. Review & Help Others
- Review pull requests
- Answer questions in issues
- Share your experience
- Provide constructive feedback

## 🔄 Contribution Workflow

### For First-Time Contributors:

1. **Fork the Repository**
```bash
   # Click "Fork" on GitHub, then clone your fork
   git clone https://github.com/YOUR-USERNAME/kubernetes-labs-collaborative.git
   cd kubernetes-labs-collaborative
```

2. **Create a Branch**
```bash
   # For solutions
   git checkout -b solution/lab-01-your-name
   
   # For documentation improvements
   git checkout -b docs/improve-lab-02
   
   # For bug fixes
   git checkout -b fix/lab-03-typo
```

3. **Make Your Changes**
   - Follow existing file structure
   - Test commands before submitting
   - Add comments to YAML files
   - Keep changes focused

4. **Commit Your Changes**
```bash
   git add .
   git commit -m "type: clear description"
```
   
   Commit message types:
   - `docs:` Documentation changes
   - `solution:` Lab solution submission
   - `fix:` Bug fixes or corrections
   - `feat:` New features or challenges
   - `refactor:` Restructuring without changing functionality

5. **Push and Create PR**
```bash
   git push origin your-branch-name
```
   Then open a Pull Request on GitHub

## 📝 Contribution Guidelines

### For Lab Solutions:

Create a new folder in the lab's solutions directory:
```
labs/01-pods/solutions/your-github-username/
├── nginx-pod.yaml
├── README.md  (explain your approach)
└── notes.md   (optional: learnings & gotchas)
```

### For Documentation:

- Use clear, simple language
- Include command examples
- Add expected output when helpful
- Test all commands before submitting

### For YAML Files:
```yaml
# Good: Include comments explaining key concepts
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx  # Labels help identify and group Pods
spec:
  containers:
  - name: nginx
    image: nginx:1.21.1  # Specific version for reproducibility
    ports:
    - containerPort: 80
```

## ✅ Pull Request Checklist

Before submitting, ensure:

- [ ] Commands have been tested
- [ ] YAML files are valid (`kubectl apply --dry-run=client`)
- [ ] Documentation is clear and helpful
- [ ] Commit messages are descriptive
- [ ] No sensitive information (passwords, keys) included
- [ ] Changes are focused (one improvement per PR)

## 🚫 What NOT to Include

- Personal credentials or tokens
- Cluster-specific configurations
- Large binary files
- Duplicate content
- Incomplete work (use draft PRs instead)

## 🤔 Questions?

- Check [existing issues](../../issues)
- Open a new issue for discussion
- Tag maintainers for guidance

## 🏆 Recognition

All contributors will be:
- Listed in our Contributors section
- Acknowledged in release notes
- Part of the learning community

Thank you for helping others learn Kubernetes! 🚀
