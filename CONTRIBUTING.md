# Contributing to LostLK

Thank you for your interest in contributing to LostLK! This document provides guidelines and information for contributors.

## 🚀 Getting Started

### Prerequisites
- Android Studio Arctic Fox or later
- Basic knowledge of Kotlin and Android development
- Git installed on your system
- Firebase account (for testing)

### Setting Up Development Environment

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/yourusername/LostLK.git
   cd LostLK
   ```
3. Set up Firebase:
   - Create a Firebase project
   - Add `google-services.json` to the `app/` directory
   - Configure Firebase Authentication and Realtime Database
4. Open the project in Android Studio
5. Sync the project with Gradle files

## 🛠️ Development Guidelines

### Code Style
- Follow Kotlin coding conventions
- Use meaningful variable and function names
- Add comments for complex logic
- Keep functions small and focused
- Use proper indentation (4 spaces)

### Commit Messages
Use clear, descriptive commit messages:
```
feat: add barcode scanning functionality
fix: resolve map marker clustering issue
docs: update README with new features
style: improve UI design consistency
refactor: optimize database queries
```

### Pull Request Process

1. **Create a Feature Branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make Your Changes**
   - Write clean, well-documented code
   - Add tests if applicable
   - Update documentation as needed

3. **Test Your Changes**
   - Test on different screen sizes
   - Verify functionality works as expected
   - Check for any linting errors

4. **Commit Your Changes**
   ```bash
   git add .
   git commit -m "feat: add your feature description"
   ```

5. **Push to Your Fork**
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create a Pull Request**
   - Go to the original repository on GitHub
   - Click "New Pull Request"
   - Select your branch
   - Fill out the PR template
   - Submit the pull request

## 🐛 Reporting Issues

When reporting issues, please include:

- **Description**: Clear description of the issue
- **Steps to Reproduce**: Detailed steps to reproduce the bug
- **Expected Behavior**: What you expected to happen
- **Actual Behavior**: What actually happened
- **Screenshots**: If applicable, include screenshots
- **Device Information**: Android version, device model
- **App Version**: Version of the app you're using

## ✨ Feature Requests

For feature requests, please:

- Check if the feature already exists
- Provide a clear description of the feature
- Explain why this feature would be useful
- Consider the impact on existing functionality

## 🧪 Testing

### Manual Testing
- Test on different Android versions (API 24+)
- Test on different screen sizes
- Test with different network conditions
- Test admin and regular user flows

### Automated Testing
- Write unit tests for business logic
- Add UI tests for critical user flows
- Ensure all tests pass before submitting PR

## 📱 UI/UX Guidelines

### Design Principles
- Follow Material Design 3 guidelines
- Ensure accessibility compliance
- Maintain consistency with existing design
- Use appropriate colors and typography

### Responsive Design
- Test on phones and tablets
- Ensure proper scaling
- Handle different orientations

## 🔒 Security Considerations

- Never commit sensitive data (API keys, passwords)
- Use Firebase security rules properly
- Validate user inputs
- Follow Android security best practices

## 📚 Documentation

### Code Documentation
- Add KDoc comments for public functions
- Document complex algorithms
- Include usage examples where helpful

### User Documentation
- Update README.md for new features
- Add screenshots for UI changes
- Update setup instructions if needed

## 🏷️ Version Control

### Branch Naming
- `feature/feature-name` - New features
- `fix/bug-description` - Bug fixes
- `docs/documentation-update` - Documentation changes
- `refactor/code-improvement` - Code refactoring

### Git Workflow
1. Always work on feature branches
2. Keep commits atomic and focused
3. Rebase before creating PR
4. Use meaningful commit messages

## 🎯 Areas for Contribution

### High Priority
- [ ] Barcode scanning implementation
- [ ] Push notification system
- [ ] Offline support with Room database
- [ ] Performance optimizations
- [ ] Accessibility improvements

### Medium Priority
- [ ] Multi-language support
- [ ] Advanced search functionality
- [ ] Image upload and management
- [ ] User rating system
- [ ] Social sharing features

### Low Priority
- [ ] Dark theme support
- [ ] Widget support
- [ ] Wear OS companion app
- [ ] Advanced analytics
- [ ] Export functionality

## 💬 Communication

### Getting Help
- Check existing issues and discussions
- Join our community discussions
- Ask questions in GitHub Discussions
- Contact maintainers for urgent issues

### Code Reviews
- Be constructive and respectful
- Focus on code quality and functionality
- Provide specific feedback
- Be open to suggestions and improvements

## 🏆 Recognition

Contributors will be recognized in:
- README.md contributors section
- Release notes
- Project documentation
- Community acknowledgments

## 📋 Checklist for Contributors

Before submitting a PR, ensure:

- [ ] Code follows project style guidelines
- [ ] All tests pass
- [ ] Documentation is updated
- [ ] No sensitive data is committed
- [ ] Changes are properly tested
- [ ] Commit messages are clear
- [ ] PR description is complete

## 🤝 Code of Conduct

### Our Pledge
We are committed to providing a welcoming and inclusive environment for all contributors.

### Expected Behavior
- Use welcoming and inclusive language
- Be respectful of differing viewpoints
- Accept constructive criticism gracefully
- Focus on what's best for the community

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or inflammatory comments
- Personal attacks or political discussions
- Spam or off-topic discussions

## 📞 Contact

- **Project Maintainer**: [Your Name]
- **Email**: [your-email@example.com]
- **GitHub**: [@yourusername]

Thank you for contributing to LostLK! 🎉
