
# UPDATED: Race Calendar Structure for Regular & Sprint Weekends

## Sprint Weekend Format Difference

### Regular Race Weekend:
- Friday: FP1, FP2
- Saturday: FP3, Qualifying
- Sunday: Race

### Sprint Race Weekend:
- Friday: FP1, Sprint Qualifying
- Saturday: Sprint Race, Qualifying
- Sunday: Race

---

## UPDATED ROW 21: Race Calendar Headers

### Recommended Structure (Explicit Columns)

```
RaceWeekend  GrandPrix  WeekendType  FP1DateTime  FP2DateTime  FP3DateTime  SprintQualifyingDateTime  SprintRaceDateTime  QualifyingDateTime  RaceDateTime  ContentTrigger1  ContentTrigger2  AutoGenerate  Status
```

**Column Details:**
- **Column A:** RaceWeekend (Round 1, Round 2, etc.)
- **Column B:** GrandPrix (Bahrain GP, Saudi Arabia GP, etc.)
- **Column C:** WeekendType (Regular / Sprint)
- **Column D:** FP1DateTime
- **Column E:** FP2DateTime (empty for sprint weekends)
- **Column F:** FP3DateTime (empty for sprint weekends)
- **Column G:** SprintQualifyingDateTime (empty for regular weekends)
- **Column H:** SprintRaceDateTime (empty for regular weekends)
- **Column I:** QualifyingDateTime
- **Column J:** RaceDateTime
- **Column K:** ContentTrigger1 (Main Race + 6hrs)
- **Column L:** ContentTrigger2 (Sprint + 6hrs, if applicable)
- **Column M:** AutoGenerate (YES/NO)
- **Column N:** Status

---

## 📋 **Example Data:**

### Regular Weekend (Bahrain GP):
```
Round 1 | Bahrain GP | Regular | 2026-03-01 14:00 | 2026-03-01 17:30 | 2026-03-02 13:00 | [empty] | [empty] | 2026-03-02 16:00 | 2026-03-03 15:00 | [formula] | [empty] | YES | SCHEDULED
```

### Sprint Weekend (China GP):
```
Round 4 | China GP | Sprint | 2026-04-19 11:30 | [empty] | [empty] | 2026-04-19 15:30 | 2026-04-20 11:00 | 2026-04-20 15:00 | 2026-04-21 15:00 | [formula] | [formula] | YES | SCHEDULED
```

---

## ✨ **Content Trigger Formulas**

**ContentTrigger1 (Column K):** Always race + 6 hours
```
=J22+TIME(6,0,0)
```

**ContentTrigger2 (Column L):** Sprint race + 6 hours (only for sprint weekends)
```
=IF(C22="Sprint", H22+TIME(6,0,0), "")
```

This means:
- **Regular weekends:** Only 1 trigger (after race)
- **Sprint weekends:** 2 triggers (after sprint race + after main race)

---

## 🤖 **Workflow 6 Logic (Future)**

When we build the race calendar auto-trigger workflow:

```javascript
// Check weekend type
if (weekendType === 'Sprint') {
  // Sprint weekend - check both triggers
  const triggers = [
    { event: 'Sprint Race', datetime: contentTrigger2 },
    { event: 'Main Race', datetime: contentTrigger1 }
  ];
} else {
  // Regular weekend - check only main race
  const triggers = [
    { event: 'Race', datetime: contentTrigger1 }
  ];
}

// For each trigger, check if it's time to generate content
triggers.forEach(trigger => {
  if (currentTime >= trigger.datetime && autoGenerate === 'YES') {
    createHighPriorityTopic(`${trigger.event} - ${grandPrix}`);
  }
});
```

---

## 📊 **2026 F1 Sprint Weekends**

For reference, typical sprint race weekends (may vary):
1. **China GP** (Shanghai) - Round 4
2. **Miami GP** - Round 6
3. **Austria GP** (Red Bull Ring) - Round 11
4. **USA GP** (COTA) - Round 19
5. **Brazil GP** (Interlagos) - Round 21
6. **Qatar GP** (Losail) - Round 23

---

## 🎯 **Why This Structure Works:**

✅ **Flexibility:** Handles both weekend types in same sheet  
✅ **Clear:** Empty cells make it obvious which format each weekend uses  
✅ **Automation-ready:** Easy to parse in n8n workflows  
✅ **Future-proof:** Can add more session types if F1 changes format  
✅ **Multiple triggers:** Sprint weekends get 2 content opportunities  

---

## ✅ **Action Checklist:**

For your Master Control sheet:

- [ ] 1. Update Row 21 headers to include all columns (14 columns total)
- [ ] 2. Add WeekendType column (Column C)
- [ ] 3. For regular weekends: Fill FP1, FP2, FP3, leave Sprint columns empty
- [ ] 4. For sprint weekends: Fill FP1, SprintQualifying, SprintRace, leave FP2/FP3 empty
- [ ] 5. Set up ContentTrigger1 formula: `=J22+TIME(6,0,0)`
- [ ] 6. Set up ContentTrigger2 formula: `=IF(C22="Sprint", H22+TIME(6,0,0), "")`
- [ ] 7. Mark sprint weekends with "Sprint" in WeekendType column
- [ ] 8. Mark regular weekends with "Regular" in WeekendType column

---

**This structure won't affect Workflow 1** (which we're testing now), but it sets you up perfectly for Workflow 6 when we build the race-based auto-triggers later!

Good catch on the sprint weekends - this makes the system much more robust! 🏎️
