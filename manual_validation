function main() {
  // open the spreadsheet using the unique ID
  // get the target range which is "category"
  // get the rules based on the adjacent column
  // apply the rules

  var spreadsheet = SpreadsheetApp.openById(""); // input the ID of the spreadsheet you are working with
  var targetRange = spreadsheet.getRangeByName("Category");
  var ranges = getRanges(spreadsheet);

  sheetApplyRules(spreadsheet, targetRange, ranges);
}

function getRanges(spreadsheet) {
  /// This function returns the name of the named range that corresponds with the value in the general column.

  // get the values in the ranges "General" and "Budget"

  var generalRange = spreadsheet.getRangeByName("General");
  var generalValues = generalRange.getValues();
  
  var accountRange = spreadsheet.getRangeByName("Budget");
  var accountValues = accountRange.getValues();

  // iterate through the values in the general column
  // if the general value is a number, then the ranges will record "g_#"
  // if the general value is in the specified list, then a code to reference that list will be recorded
  // if neither of those requirements apply then it records an empty line

  var ranges = [];

  for (var i = 0; i < generalValues.length; i++) {
    var generalValue = generalValues[i];
    var accountValue = String(accountValues[i]);

    // if (generalValue != '' && generalValue != '#') {
    // this is a a different way of checking the boolean

    if (!isNaN(generalValue)) {
      var name = "g_" + generalValue;
      ranges.push(name);

    } else if (!["Balance", "————", "Refund", "Repay", "Budget"].includes(accountValue)) {
      var name = accountValue.toLowerCase().replace(/ /g, "_");
      ranges.push(name);

    } else {
      ranges.push("");
    }
  }
  return ranges;
}

function sheetApplyRules(spreadsheet, targetRange, ranges) {
  for (var i = 0; i < targetRange.getValues().length; i++) {
    var target = targetRange.getCell(i+1,1);
    var name = ranges[i];

    if (name != '') { 
      var rangeValues = spreadsheet.getRangeByName(name).getValues()

      var rule = SpreadsheetApp.newDataValidation()
      .requireValueInList(rangeValues)
      .build()

      target.setDataValidation(rule);
    }
  }
}
