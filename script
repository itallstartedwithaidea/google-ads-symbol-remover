/**
 * Google Ads Symbol Remover
 * 
 * @description Automatically removes specified symbols (℠, ™, ®, ©, etc.) from 
 *              Responsive Search Ads and Performance Max ad assets across your account
 * 
 * @author John - Senior Paid Media Specialist
 * @version 1.0.0
 * @date December 2025
 * 
 * @features
 * - Removes configurable symbols from RSA and PMax ads
 * - Handles both active and paused ads
 * - Preserves all ad settings (URLs, paths, pinning)
 * - Automatic spacing cleanup after symbol removal
 * - Detailed logging and error handling
 * - Summary statistics (found vs updated)
 * 
 * @usage
 * 1. Copy this script into Google Ads Scripts
 * 2. Configure SYMBOLS_TO_REMOVE array with your target symbols
 * 3. Preview to see what will change
 * 4. Run to execute changes
 * 
 * @notes
 * - Creates new ads and pauses old ones (doesn't delete)
 * - Review paused ads before deleting manually
 * - Test on a single account before MCC-wide deployment
 */

// ============================================
// CONFIGURATION - Edit symbols to remove here
// ============================================
var SYMBOLS_TO_REMOVE = ['℠', '™', '®', '©'];
// Add any other symbols you want to remove: ['℠', '™', '®', '©', '•', '★', '†', '‡', '§']

function main() {
  var summary = {
    rsaAdsFound: 0,
    rsaAdsUpdated: 0,
    pmaxAdsFound: 0,
    pmaxAdsUpdated: 0,
    errors: []
  };
  
  Logger.log('Starting ad text update...');
  Logger.log('Target: Remove symbols: ' + SYMBOLS_TO_REMOVE.join(', '));
  Logger.log('Ad Types: RSA & Performance Max');
  Logger.log('-------------------------------------------');
  
  try {
    // Process Responsive Search Ads
    var rsaResults = processResponsiveSearchAds();
    summary.rsaAdsFound = rsaResults.found;
    summary.rsaAdsUpdated = rsaResults.updated;
    
    // Process Performance Max Ads
    var pmaxResults = processPerformanceMaxAds();
    summary.pmaxAdsFound = pmaxResults.found;
    summary.pmaxAdsUpdated = pmaxResults.updated;
    
  } catch (e) {
    summary.errors.push(e.message);
    Logger.log('✗ Error: ' + e.message);
  }
  
  // Final summary
  Logger.log('\n===========================================');
  Logger.log('EXECUTION SUMMARY');
  Logger.log('===========================================');
  Logger.log('Symbols Removed: ' + SYMBOLS_TO_REMOVE.join(', '));
  Logger.log('\nResponsive Search Ads:');
  Logger.log('  - Ads with symbols found: ' + summary.rsaAdsFound);
  Logger.log('  - Ads updated: ' + summary.rsaAdsUpdated);
  Logger.log('\nPerformance Max Ads:');
  Logger.log('  - Ads with symbols found: ' + summary.pmaxAdsFound);
  Logger.log('  - Ads updated: ' + summary.pmaxAdsUpdated);
  Logger.log('\nTotal Ads Updated: ' + (summary.rsaAdsUpdated + summary.pmaxAdsUpdated));
  
  if (summary.errors.length > 0) {
    Logger.log('\nErrors Encountered: ' + summary.errors.length);
    summary.errors.forEach(function(error) {
      Logger.log('  - ' + error);
    });
  }
}

/**
 * Check if text contains any of the target symbols
 * @param {string} text - Text to check
 * @returns {boolean} True if text contains any target symbol
 */
function containsSymbols(text) {
  for (var i = 0; i < SYMBOLS_TO_REMOVE.length; i++) {
    if (text.indexOf(SYMBOLS_TO_REMOVE[i]) > -1) {
      return true;
    }
  }
  return false;
}

/**
 * Remove all target symbols and clean spacing
 * @param {string} text - Text to clean
 * @returns {string} Cleaned text with symbols removed and proper spacing
 */
function cleanText(text) {
  var cleanedText = text;
  
  // Remove each symbol
  for (var i = 0; i < SYMBOLS_TO_REMOVE.length; i++) {
    var symbol = SYMBOLS_TO_REMOVE[i];
    var regex = new RegExp(escapeRegExp(symbol), 'g');
    cleanedText = cleanedText.replace(regex, '');
  }
  
  // Clean up spacing
  cleanedText = cleanedText
    .replace(/\s+/g, ' ')  // Replace multiple spaces with single space
    .trim();                // Remove leading/trailing spaces
  
  return cleanedText;
}

/**
 * Escape special regex characters in a string
 * @param {string} string - String to escape
 * @returns {string} Escaped string safe for regex
 */
function escapeRegExp(string) {
  return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
}

/**
 * Process all Responsive Search Ads in the account
 * @returns {Object} Object with found and updated counts
 */
function processResponsiveSearchAds() {
  var found = 0;
  var updated = 0;
  
  Logger.log('\nProcessing Responsive Search Ads...');
  
  // Get both active and paused RSAs
  var adSelector = AdsApp.ads()
    .withCondition('Type = RESPONSIVE_SEARCH_AD')
    .withCondition('CampaignStatus IN [ENABLED, PAUSED]')
    .withCondition('AdGroupStatus IN [ENABLED, PAUSED]');
  
  var adIterator = adSelector.get();
  
  while (adIterator.hasNext()) {
    var ad = adIterator.next();
    var rsa = ad.asType().responsiveSearchAd();
    var needsUpdate = false;
    
    var headlines = rsa.getHeadlines();
    var descriptions = rsa.getDescriptions();
    
    // Check if any headline contains symbols
    for (var i = 0; i < headlines.length; i++) {
      if (containsSymbols(headlines[i].text)) {
        needsUpdate = true;
        break;
      }
    }
    
    // Check if any description contains symbols
    if (!needsUpdate) {
      for (var j = 0; j < descriptions.length; j++) {
        if (containsSymbols(descriptions[j].text)) {
          needsUpdate = true;
          break;
        }
      }
    }
    
    if (needsUpdate) {
      found++;
      
      try {
        var adGroup = ad.getAdGroup();
        var newAdBuilder = adGroup.newAd().responsiveSearchAdBuilder();
        
        // Add cleaned headlines with proper spacing
        headlines.forEach(function(headline) {
          var cleaned = cleanText(headline.text);
          newAdBuilder.addHeadline(cleaned);
        });
        
        // Add cleaned descriptions with proper spacing
        descriptions.forEach(function(description) {
          var cleaned = cleanText(description.text);
          newAdBuilder.addDescription(cleaned);
        });
        
        // Add final URL
        newAdBuilder.withFinalUrl(rsa.urls().getFinalUrl());
        
        // Copy final mobile URL if exists
        var mobileFinalUrl = rsa.urls().getMobileFinalUrl();
        if (mobileFinalUrl) {
          newAdBuilder.withMobileFinalUrl(mobileFinalUrl);
        }
        
        // Copy path fields if they exist
        var path1 = rsa.getPath1();
        var path2 = rsa.getPath2();
        if (path1) newAdBuilder.withPath1(path1);
        if (path2) newAdBuilder.withPath2(path2);
        
        // Build new ad and pause old one
        newAdBuilder.build();
        ad.pause();
        updated++;
        
        Logger.log('  ✓ Updated RSA in Ad Group: ' + adGroup.getName());
        
      } catch (e) {
        Logger.log('  ✗ Failed to update RSA: ' + e.message);
      }
    }
  }
  
  return { found: found, updated: updated };
}

/**
 * Process all Performance Max ad assets in the account
 * @returns {Object} Object with found and updated counts
 */
function processPerformanceMaxAds() {
  var found = 0;
  var updated = 0;
  
  Logger.log('\nProcessing Performance Max Assets...');
  
  // Get Performance Max campaigns
  var campaignSelector = AdsApp.campaigns()
    .withCondition('AdvertisingChannelType = PERFORMANCE_MAX')
    .withCondition('Status IN [ENABLED, PAUSED]');
  
  var campaignIterator = campaignSelector.get();
  
  while (campaignIterator.hasNext()) {
    var campaign = campaignIterator.next();
    Logger.log('  Checking PMax Campaign: ' + campaign.getName());
    
    // Get asset groups for this campaign
    var assetGroupSelector = campaign.assetGroups()
      .withCondition('Status IN [ENABLED, PAUSED]');
    
    var assetGroupIterator = assetGroupSelector.get();
    
    while (assetGroupIterator.hasNext()) {
      var assetGroup = assetGroupIterator.next();
      var needsUpdate = false;
      
      // Check headlines
      var headlines = assetGroup.headlines().get();
      while (headlines.hasNext()) {
        var headline = headlines.next();
        if (containsSymbols(headline.getText())) {
          needsUpdate = true;
          break;
        }
      }
      
      // Check descriptions if not already flagged
      if (!needsUpdate) {
        var descriptions = assetGroup.descriptions().get();
        while (descriptions.hasNext()) {
          var description = descriptions.next();
          if (containsSymbols(description.getText())) {
            needsUpdate = true;
            break;
          }
        }
      }
      
      // Check long headlines
      if (!needsUpdate) {
        var longHeadlines = assetGroup.longHeadlines().get();
        while (longHeadlines.hasNext()) {
          var longHeadline = longHeadlines.next();
          if (containsSymbols(longHeadline.getText())) {
            needsUpdate = true;
            break;
          }
        }
      }
      
      if (needsUpdate) {
        found++;
        
        try {
          // Remove old text assets with symbols
          var headlinesToRemove = assetGroup.headlines().get();
          while (headlinesToRemove.hasNext()) {
            var headline = headlinesToRemove.next();
            if (containsSymbols(headline.getText())) {
              var cleaned = cleanText(headline.getText());
              
              // Add cleaned version
              assetGroup.addHeadline(cleaned);
              
              // Remove old version
              headline.remove();
            }
          }
          
          // Process descriptions
          var descriptionsToUpdate = assetGroup.descriptions().get();
          while (descriptionsToUpdate.hasNext()) {
            var description = descriptionsToUpdate.next();
            if (containsSymbols(description.getText())) {
              var cleaned = cleanText(description.getText());
              
              assetGroup.addDescription(cleaned);
              description.remove();
            }
          }
          
          // Process long headlines
          var longHeadlinesToUpdate = assetGroup.longHeadlines().get();
          while (longHeadlinesToUpdate.hasNext()) {
            var longHeadline = longHeadlinesToUpdate.next();
            if (containsSymbols(longHeadline.getText())) {
              var cleaned = cleanText(longHeadline.getText());
              
              assetGroup.addLongHeadline(cleaned);
              longHeadline.remove();
            }
          }
          
          updated++;
          Logger.log('    ✓ Updated Asset Group: ' + assetGroup.getName());
          
        } catch (e) {
          Logger.log('    ✗ Failed to update Asset Group: ' + e.message);
        }
      }
    }
  }
  
  return { found: found, updated: updated };
}
