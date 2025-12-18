# Google Ads Symbol Remover

Automatically remove trademark symbols (℠, ™, ®, ©) and other special characters from your Google Ads Responsive Search Ads and Performance Max campaigns.

## 🎯 Purpose

When trademark requirements change or you need to clean up ad copy across large accounts, manually editing hundreds or thousands of ads is time-consuming and error-prone. This script automates the process while preserving all other ad settings.

## ✨ Features

- ✅ Removes configurable symbols from ad text
- ✅ Supports Responsive Search Ads (RSA)
- ✅ Supports Performance Max ad assets
- ✅ Processes both active and paused ads
- ✅ Preserves URLs, paths, and all other settings
- ✅ Automatic spacing cleanup after removal
- ✅ Detailed logging with found vs. updated counts
- ✅ Safe approach: creates new ads, pauses old ones
- ✅ Comprehensive error handling

## 🚀 Quick Start

### Installation

1. Log into your Google Ads account
2. Navigate to **Tools & Settings > Bulk Actions > Scripts**
3. Click the **+** button to create a new script
4. Copy the contents of `symbol-remover.js`
5. Paste into the script editor
6. Save with a descriptive name (e.g., "Remove Trademark Symbols")

### Configuration

Edit the `SYMBOLS_TO_REMOVE` array at the top of the script:
```javascript
var SYMBOLS_TO_REMOVE = ['℠', '™', '®', '©'];
```

**Common symbols you might want to remove:**
- `℠` - Service mark
- `™` - Trademark
- `®` - Registered trademark
- `©` - Copyright
- `•` - Bullet point
- `★` - Star
- `†` - Dagger
- `‡` - Double dagger
- `§` - Section sign

### Usage

1. **Preview First**: Click "Preview" to see what will change without making actual changes
2. **Review Logs**: Check the log output to see how many ads will be affected
3. **Run Script**: Click "Run" to execute the changes
4. **Review Results**: Check the execution summary in the logs

### Example Output
```
Starting ad text update...
Target: Remove symbols: ℠, ™, ®, ©
Ad Types: RSA & Performance Max
-------------------------------------------

Processing Responsive Search Ads...
  ✓ Updated RSA in Ad Group: Brand - Exact Match
  ✓ Updated RSA in Ad Group: Services - General

Processing Performance Max Assets...
  Checking PMax Campaign: Performance Max - All Products
    ✓ Updated Asset Group: Default Asset Group

===========================================
EXECUTION SUMMARY
===========================================
Symbols Removed: ℠, ™, ®, ©

Responsive Search Ads:
  - Ads with symbols found: 47
  - Ads updated: 47

Performance Max Ads:
  - Ads with symbols found: 12
  - Ads updated: 12

Total Ads Updated: 59
```

## 🔧 How It Works

### Responsive Search Ads (RSA)
1. Scans all RSAs (active and paused) for target symbols
2. Creates new ads with cleaned text
3. Pauses original ads (keeps them for reference)
4. Preserves all settings: URLs, mobile URLs, path fields

### Performance Max
1. Scans all asset groups for target symbols in:
   - Headlines
   - Long headlines
   - Descriptions
2. Adds cleaned versions of text assets
3. Removes old assets with symbols
4. Preserves all other asset group settings

### Text Cleaning Process
- Removes all specified symbols
- Collapses multiple spaces into single space
- Trims leading and trailing whitespace
- Example: `"Children's Health℠  "` → `"Children's Health"`

## ⚠️ Important Notes

### Before Running

- **Test on one account first** before deploying MCC-wide
- **Use Preview mode** to see changes without executing
- **Review the logs** to understand what will change

### After Running

- **Review paused ads** to ensure changes are correct
- **Delete old paused ads manually** once you've verified the new ones
- **Monitor performance** of new ads for 1-2 weeks

### Limitations

- Does not process legacy Expanded Text Ads (ETAs)
- Creates new ads rather than editing in place
- Old ads are paused, not deleted (manual cleanup required)

## 🎓 Use Cases

### Trademark Compliance
Remove trademark symbols when legal requirements change or when trademark registrations expire.
```javascript
var SYMBOLS_TO_REMOVE = ['℠', '™', '®'];
```

### Brand Guidelines Update
Clean up ads to match new brand guidelines that prohibit special characters.
```javascript
var SYMBOLS_TO_REMOVE = ['℠', '™', '®', '©', '•', '★'];
```

### Character Limit Optimization
Remove symbols to free up character space for additional messaging.
```javascript
var SYMBOLS_TO_REMOVE = ['℠', '™', '®', '©', '†', '‡'];
```

## 📊 Best Practices

1. **Start Small**: Test on a single campaign or account first
2. **Document Changes**: Keep a log of which symbols were removed and when
3. **Monitor Performance**: Track CTR and conversion changes after deployment
4. **Keep Old Ads**: Don't delete paused ads immediately - wait 30 days
5. **Run Off-Hours**: Schedule during low-traffic periods to minimize disruption

## 🔍 Troubleshooting

### "No ads updated" but symbols exist
- Check that campaigns/ad groups are enabled or paused (not removed)
- Verify symbols are in the `SYMBOLS_TO_REMOVE` array
- Ensure you have edit permissions

### Script timeout errors
- Reduce batch size by running on specific campaigns first
- Use MCC-level scripts to distribute load across accounts

### Spacing issues after removal
- The script automatically handles spacing cleanup
- If issues persist, check for non-standard space characters

## 🤝 Contributing

This script was developed for enterprise-scale Google Ads management. Contributions and improvements are welcome!

### To Contribute
1. Fork the repository
2. Create a feature branch
3. Test thoroughly on real accounts
4. Submit a pull request with detailed explanation

## 📝 Version History

### v1.0.0 (December 2025)
- Initial release
- RSA and Performance Max support
- Configurable symbol removal
- Automatic spacing cleanup
- Comprehensive logging

## 👤 Author

**John** - Senior Paid Media Specialist  
15+ years managing large-scale advertising campaigns  
Specializing in automation and AI-powered paid media optimization

## 📄 License

MIT License - Feel free to use and modify for your needs

## 💡 Related Projects

Looking for more Google Ads automation? Check out:
- Search query optimization tools
- Automated RSA generation
- Budget pacing dashboards
- Creative performance analyzers

---

**Questions or Issues?** Open an issue in this repository or connect with me on LinkedIn.

**Found this helpful?** Star the repo and share with other paid media professionals!
```

**3. LICENSE**
```
MIT License

Copyright (c) 2025 John

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

**4. .gitignore**
```
# Logs
logs
*.log
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Editor directories and files
.vscode/
.idea/
*.swp
*.swo
*~
.DS_Store

# Test files
test/
*.test.js

# Environment variables
.env
.env.local
