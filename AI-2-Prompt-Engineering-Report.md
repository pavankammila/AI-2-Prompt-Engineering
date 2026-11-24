# AI-2: Prompt Engineering — Experimental Report

**Student:** Kammila Pavan Kumar  
**Course:** Artificial Intelligence  
**Assignment:** AI-2 – Prompt Engineering  
**Experimental Model:** ChatGPT (exact model version not recorded)  
**Tools:** ChatGPT, Markdown  
**Experiments:** Zero-shot vs. Few-shot, Chain-of-Thought, Structured JSON Output

---

## 1. Introduction

Prompt engineering is the process of designing and optimizing instructions given to Large Language Models (LLMs) to obtain accurate, consistent, and structured responses.

This assignment explores three important prompt-engineering techniques:

1. Zero-shot vs. Few-shot prompting.
2. Chain-of-Thought prompting.
3. Structured JSON output.

The objective was to conduct practical experiments, compare different prompting approaches, evaluate their results, and understand how prompt design influences model behaviour.

## 2. Experimental Setup

| Parameter | Details |
|---|---|
| Experimental model | ChatGPT |
| Model version | Not recorded |
| Interface | ChatGPT |
| Programming required | No |
| Experiment 1 dataset | 5 customer feedback statements |
| Experiment 2 dataset | 1 multi-step mathematical problem |
| Experiment 3 dataset | 10 invoice extraction inputs |
| Evaluation | Accuracy, consistency, correctness, schema compliance |

**Experimental note:** Google AI Studio – Gemini was initially selected during planning. However, the actual experiments reported here were conducted using ChatGPT. Therefore, results are attributed to ChatGPT rather than Gemini.

---

# 3. Experiment 1: Zero-Shot vs. Few-Shot Prompting

## 3.1 Objective

To compare the performance of zero-shot and few-shot prompting in classifying customer feedback into Positive, Negative, and Neutral categories.

## 3.2 Test Dataset

| ID | Customer Feedback | Expected Label |
|---|---|---|
| T1 | The product quality is excellent. I'm extremely happy with my purchase! | Positive |
| T2 | The delivery was late, and customer support was completely unhelpful. | Negative |
| T3 | The package arrived yesterday. I haven't used the product yet. | Neutral |
| T4 | I expected much better quality for this price. Very disappointed. | Negative |
| T5 | It's reasonably good and does exactly what I expected. | Positive |

## 3.3 Experiment A: Zero-Shot Prompting

**Prompt used:**

```text
You are a professional sentiment classification system.

TASK:
Classify each customer feedback statement into exactly one of these categories:
- Positive
- Negative
- Neutral

RULES:
1. Positive: The customer expresses satisfaction, happiness, or approval.
2. Negative: The customer expresses dissatisfaction, frustration, or disappointment.
3. Neutral: The customer provides factual information without a clear positive or negative opinion.
4. Do not invent information or explanations.
5. Return exactly one label per input.
6. Follow the output format strictly.

OUTPUT FORMAT:
T1: <label>
T2: <label>
T3: <label>
T4: <label>
T5: <label>

CUSTOMER FEEDBACK:

T1: "The product quality is excellent. I'm extremely happy with my purchase!"

T2: "The delivery was late, and customer support was completely unhelpful."

T3: "The package arrived yesterday. I haven't used the product yet."

T4: "I expected much better quality for this price. Very disappointed."

T5: "It's reasonably good and does exactly what I expected."

Return only the five classifications.
```

**Actual raw model output:**

```text
T1: Positive
T2: Negative
T3: Neutral
T4: Negative
T5: Positive
```

## 3.4 Experiment B: Few-Shot Prompting

**Prompt used:**

```text
You are a professional sentiment classification system.

TASK:
Classify each customer feedback statement into exactly one category:
Positive, Negative, or Neutral.

LEARN FROM THESE EXAMPLES:

Example 1:
Feedback: "I love this product. It works perfectly!"
Label: Positive

Example 2:
Feedback: "Terrible experience. The product stopped working after one day."
Label: Negative

Example 3:
Feedback: "The order was placed on Monday and delivered on Wednesday."
Label: Neutral

Example 4:
Feedback: "The service was good, but I am not completely satisfied."
Label: Negative

CLASSIFICATION RULES:
1. Positive = clear satisfaction or approval.
2. Negative = clear dissatisfaction or criticism, even if mixed with some positive feedback.
3. Neutral = factual statements without clear sentiment.
4. Apply the same classification logic consistently.
5. Do not copy labels from examples blindly.
6. Return exactly one label per test input.

OUTPUT FORMAT:
T1: <label>
T2: <label>
T3: <label>
T4: <label>
T5: <label>

CUSTOMER FEEDBACK:

T1: "The product quality is excellent. I'm extremely happy with my purchase!"

T2: "The delivery was late, and customer support was completely unhelpful."

T3: "The package arrived yesterday. I haven't used the product yet."

T4: "I expected much better quality for this price. Very disappointed."

T5: "It's reasonably good and does exactly what I expected."

Return only the five classifications.
```

**Actual raw model output:**

```text
T1: Positive
T2: Negative
T3: Neutral
T4: Negative
T5: Positive
```

## 3.5 Results and Comparison

| Test | Expected | Zero-Shot | Few-Shot |
|---|---|---|---|
| T1 | Positive | Positive | Positive |
| T2 | Negative | Negative | Negative |
| T3 | Neutral | Neutral | Neutral |
| T4 | Negative | Negative | Negative |
| T5 | Positive | Positive | Positive |

**Accuracy calculation:**

Accuracy = (Correct Predictions / Total Predictions) × 100

Zero-shot:

(5 / 5) × 100 = **100%**

Few-shot:

(5 / 5) × 100 = **100%**

## 3.6 Analysis

Both prompting approaches achieved identical results, correctly classifying all five feedback statements.

The few-shot examples did not improve accuracy because the task was relatively straightforward and the zero-shot instructions were already sufficiently clear.

Both approaches followed the requested output format and produced identical classifications. However, a larger dataset containing sarcasm, ambiguous statements, and mixed sentiments would be needed to determine whether few-shot prompting offers greater reliability in more complex situations.

**Conclusion:** Few-shot prompting provides examples of expected behaviour, but it did not produce a measurable accuracy improvement in this experiment.

---

# 4. Experiment 2: Chain-of-Thought Prompting

## 4.1 Objective

To compare direct prompting with explicit step-by-step instructions on a multi-step mathematical word problem.

## 4.2 Problem Statement

A student organizing a college event purchases:

- 10 notebooks at ₹40 each.
- 6 pens at ₹15 each.
- 4 folders at ₹25 each.

The shop offers a 10% discount on the total purchase. The student also has a ₹31 discount voucher. Delivery charges are ₹40.

The student's total budget is ₹600.

Calculate:

1. Original purchase cost.
2. Cost after the percentage discount.
3. Cost after applying the voucher.
4. Final amount including delivery.
5. Remaining budget.

## 4.3 Experiment A: Direct Prompting

**Prompt used:**

```text
Solve the following mathematical word problem.

A student organizing a college event purchases:
- 10 notebooks at ₹40 each.
- 6 pens at ₹15 each.
- 4 folders at ₹25 each.

The shop offers a 10% discount on the total purchase. The student also has a ₹31 discount voucher. Delivery charges are ₹40.

The student's total budget is ₹600.

Calculate:
1. Original purchase cost.
2. Cost after the percentage discount.
3. Cost after applying the voucher.
4. Final amount including delivery.
5. Remaining budget.

Provide the final answers clearly with appropriate calculations.
```

**Actual raw model output:**

```text
### Step-by-step calculations

1. Original purchase cost

- Notebooks: 10 × ₹40 = ₹400
- Pens: 6 × ₹15 = ₹90
- Folders: 4 × ₹25 = ₹100

Total = ₹400 + ₹90 + ₹100

Original purchase cost = ₹590

2. Cost after 10% discount

10% of ₹590 = ₹59

₹590 − ₹59 = ₹531

3. Cost after applying ₹31 voucher

₹531 − ₹31 = ₹500

4. Final amount including delivery

₹500 + ₹40 = ₹540

5. Remaining budget

₹600 − ₹540 = ₹60

Final Answers

Original purchase cost: ₹590
After 10% discount: ₹531
After ₹31 voucher: ₹500
Including delivery charges: ₹540
Remaining budget: ₹60

Within budget

Conclusion: The student's final expenditure is ₹540,
leaving ₹60 from the total budget of ₹600.
```

## 4.4 Experiment B: Step-by-Step Prompting

**Prompt used:**

```text
You are a mathematical problem-solving assistant.

Solve the following word problem carefully.

A student organizing a college event purchases:
- 10 notebooks at ₹40 each.
- 6 pens at ₹15 each.
- 4 folders at ₹25 each.

The shop offers a 10% discount on the total purchase. The student also has a ₹31 discount voucher. Delivery charges are ₹40.

The student's total budget is ₹600.

INSTRUCTIONS:
1. Break the problem into logical calculation steps.
2. Calculate the cost of each item category.
3. Add all item costs to obtain the original total.
4. Calculate the 10% discount.
5. Subtract the ₹31 voucher.
6. Add the ₹40 delivery charge.
7. Calculate the remaining budget.
8. Verify that the final calculations are arithmetically consistent.

OUTPUT FORMAT:
- Original purchase cost:
- Discount amount:
- Amount after percentage discount:
- Amount after voucher:
- Delivery charges:
- Final payable amount:
- Remaining budget:

Show concise calculation steps and provide the final answer.
```

**Actual raw model output:**

```text
### Step-by-step calculations

1. Calculate individual item costs

- Notebooks: 10 × ₹40 = ₹400
- Pens: 6 × ₹15 = ₹90
- Folders: 4 × ₹25 = ₹100

2. Original purchase cost

₹400 + ₹90 + ₹100 = ₹590

3. Calculate 10% discount

10% of ₹590 = ₹590 × 10/100 = ₹59

4. Amount after percentage discount

₹590 − ₹59 = ₹531

5. Amount after voucher

₹531 − ₹31 = ₹500

6. Add delivery charges

₹500 + ₹40 = ₹540

7. Calculate remaining budget

₹600 − ₹540 = ₹60

Final Answer

Original purchase cost: ₹590
Discount amount: ₹59
Amount after percentage discount: ₹531
Amount after voucher: ₹500
Delivery charges: ₹40
Final payable amount: ₹540
Remaining budget: ₹60

Calculations verified

Conclusion: The student spends ₹540 and has ₹60 remaining.
The purchase is within the ₹600 budget.
```

## 4.5 Mathematical Verification

| Calculation | Correct Answer |
|---|---:|
| Notebooks | ₹400 |
| Pens | ₹90 |
| Folders | ₹100 |
| Original purchase cost | ₹590 |
| 10% discount | ₹59 |
| After discount | ₹531 |
| After voucher | ₹500 |
| Delivery charges | ₹40 |
| Final amount | ₹540 |
| Remaining budget | ₹60 |

## 4.6 Results

| Evaluation | Direct | Step-by-Step |
|---|---|---|
| Mathematical correctness | 100% | 100% |
| Intermediate calculations | Correct | Correct |
| Final amount | ₹540 | ₹540 |
| Remaining budget | ₹60 | ₹60 |
| Output completeness | Complete | Complete |

## 4.7 Analysis

Both approaches achieved 100% mathematical correctness.

Interestingly, the direct prompt also produced detailed calculations without explicitly asking the model to reason step by step. The step-by-step prompt generated a more structured response and followed the requested output format more closely.

There was no measurable improvement in final numerical accuracy. However, explicit step-by-step instructions improved the predictability and organization of the response.

**Conclusion:** Step-by-step prompting is useful for organizing multi-step solutions, although it does not guarantee improved accuracy in every problem.

---

# 5. Experiment 3: Structured JSON Output

## 5.1 Objective

To test whether an LLM can consistently extract structured information from unstructured invoice descriptions and return valid JSON matching a predefined schema.

## 5.2 Defined JSON Schema

```json
{
  "id": "string",
  "name": "string or null",
  "date": "YYYY-MM-DD or null",
  "amount": "number or null"
}
```

### Extraction Rules

1. Preserve the test case ID.
2. Extract the invoice sender or customer name.
3. Normalize dates into YYYY-MM-DD format.
4. Extract the total payable amount.
5. Remove currency symbols and words.
6. Preserve decimal values.
7. Use JSON null for missing information.
8. Do not invent missing values.
9. Return exactly 10 objects in a JSON array.
10. Return no additional explanations.

## 5.3 Prompt Used

```text
You are an accurate invoice information extraction system.

TASK:
Extract the invoice sender/customer name, invoice date,
and total payable amount from each unstructured text input.

OUTPUT JSON SCHEMA:
{
  "id": "string",
  "name": "string or null",
  "date": "YYYY-MM-DD or null",
  "amount": "number or null"
}

STRICT EXTRACTION RULES:

1. ID:
   Preserve the test case ID exactly.

2. NAME:
   Extract the clearly identified invoice sender or customer name.
   If no name is provided, return null.

3. DATE:
   Normalize all identifiable dates into YYYY-MM-DD format.
   Interpret dates using the surrounding context.
   If the date cannot be identified, return null.

4. AMOUNT:
   Extract the total payable amount.
   Remove currency symbols and currency words.
   Return the amount as a JSON number.
   Preserve decimal values.
   If no amount is provided, return null.

5. ACCURACY:
   Do not invent missing information.
   Do not confuse invoice IDs with amounts.
   Do not include irrelevant information.

6. OUTPUT REQUIREMENTS:
   Return exactly 10 JSON objects inside one JSON array.
   Preserve the input order.
   Every object must contain all four keys:
   id, name, date, amount.
   Use null for missing values.
   Ensure valid JSON syntax.
   Do not include comments, Markdown code fences,
   explanations, or text outside the JSON array.

TEST INPUTS:

T1: Received an invoice from Rahul Sharma on 15 November 2026.
Total payable: ₹4,500.

T2: Invoice issued by Priya Mehta, dated 2 December 2026,
amount due Rs. 7,250.

T3: Amit Verma sent invoice INV-104 on December 5, 2026
for INR 12,999.

T4: Please process the bill from Neha Patel.
It was generated on 20 Oct 2026.
The amount is ₹850.

T5: Invoice date: 2026-11-18. Total: ₹3,200.

T6: Karan Shah issued a bill for ₹6,750.

T7: Dated 10 November 2026, invoice from Sneha Rao.

T8: Kindly note: Mr. Arjun Desai's invoice dated 1 January 2026
has a final payable amount of Rs. 15,000.

T9: From: Meera Joshi.
Bill date: 2026-09-25.
Amount payable: 2,499.50 rupees.

T10: Invoice from Rohan Gupta, issued on 14 November 2026.
No total amount is mentioned in the document.

Return only the JSON array.
```

## 5.4 Actual Model Output

The following is the submitted response, with whitespace and display escaping normalized for readability. The extracted values are unchanged.

```json
[
  {
    "id": "T1",
    "name": "Rahul Sharma",
    "date": "2026-11-15",
    "amount": 4500
  },
  {
    "id": "T2",
    "name": "Priya Mehta",
    "date": "2026-12-02",
    "amount": 7250
  },
  {
    "id": "T3",
    "name": "Amit Verma",
    "date": "2026-12-05",
    "amount": 12999
  },
  {
    "id": "T4",
    "name": "Neha Patel",
    "date": "2026-10-20",
    "amount": 850
  },
  {
    "id": "T5",
    "name": null,
    "date": "2026-11-18",
    "amount": 3200
  },
  {
    "id": "T6",
    "name": "Karan Shah",
    "date": null,
    "amount": 6750
  },
  {
    "id": "T7",
    "name": "Sneha Rao",
    "date": "2026-11-10",
    "amount": null
  },
  {
    "id": "T8",
    "name": "Arjun Desai",
    "date": "2026-01-01",
    "amount": 15000
  },
  {
    "id": "T9",
    "name": "Meera Joshi",
    "date": "2026-09-25",
    "amount": 2499.50
  },
  {
    "id": "T10",
    "name": "Rohan Gupta",
    "date": "2026-11-14",
    "amount": null
  }
]
```

## 5.5 Test Results

| ID | Expected Name | Expected Date | Expected Amount | Result |
|---|---|---|---:|---|
| T1 | Rahul Sharma | 2026-11-15 | 4500 | Pass |
| T2 | Priya Mehta | 2026-12-02 | 7250 | Pass |
| T3 | Amit Verma | 2026-12-05 | 12999 | Pass |
| T4 | Neha Patel | 2026-10-20 | 850 | Pass |
| T5 | null | 2026-11-18 | 3200 | Pass |
| T6 | Karan Shah | null | 6750 | Pass |
| T7 | Sneha Rao | 2026-11-10 | null | Pass |
| T8 | Arjun Desai | 2026-01-01 | 15000 | Pass |
| T9 | Meera Joshi | 2026-09-25 | 2499.50 | Pass |
| T10 | Rohan Gupta | 2026-11-14 | null | Pass |

## 5.6 Accuracy Calculation

Record-level extraction accuracy:

(10 / 10) × 100 = **100%**

Field-level accuracy:

(40 / 40) × 100 = **100%**

Schema compliance:

(10 / 10) × 100 = **100%**

JSON syntax was assessed from the normalized submitted structure. The pasted chat representation contained display-escaping characters, so a separate machine parser validation of the original raw text was not recorded.

## 5.7 Analysis

The model successfully extracted the required invoice details from all ten test inputs.

The explicit schema and detailed instructions helped maintain a consistent output structure. The model correctly handled different date formats, currency representations, decimal amounts, and missing values.

Using null for unavailable information was particularly useful because it prevented the model from inventing missing details.

However, the dataset was relatively small. More complex invoices containing multiple dates, subtotal amounts, taxes, and competing totals would be useful for further testing.

**Conclusion:** Explicit schema definitions and missing-value rules helped achieve consistent structured extraction across all ten test cases.

---

# 6. Overall Experimental Comparison

| Experiment | Technique | Test Cases | Accuracy |
|---|---|---:|---:|
| Experiment 1A | Zero-shot | 5 | 100% |
| Experiment 1B | Few-shot | 5 | 100% |
| Experiment 2A | Direct prompting | 1 | Correct |
| Experiment 2B | Step-by-step | 1 | Correct |
| Experiment 3 | Structured JSON | 10 | 100% |

Note: The reasoning experiment used one problem per approach, so its correctness result should not be interpreted as a general statistical accuracy rate.

## 7. Key Learnings

Through these experiments, I learned:

1. **Zero-shot prompting:** Clear instructions can be sufficient for straightforward classification tasks without examples.

2. **Few-shot prompting:** Providing examples helps demonstrate expected model behaviour and can be useful for more complex or ambiguous tasks.

3. **Step-by-step prompting:** Explicit instructions can improve the organization and transparency of mathematical solutions.

4. **Structured output:** Defining a clear JSON schema and specifying missing-value behaviour helps produce consistent machine-readable responses.

5. **Prompt clarity:** Specific instructions, output formats, and constraints are important for improving response reliability.

6. **Honest evaluation:** A successful experiment should also document cases where a technique provides no measurable improvement.

## 8. Challenges and Limitations

- The experiments used relatively small datasets.
- The classification dataset contained mostly straightforward sentiment examples.
- The mathematical reasoning experiment included only one problem.
- The JSON experiment did not test complex invoices with multiple competing values.
- The exact model version and generation settings were not recorded.
- No separate automated JSON parser validation was recorded.
- Few-shot prompting did not improve accuracy over zero-shot prompting in this dataset.

These limitations mean that the results demonstrate successful performance on the selected test cases, not universal reliability.

## 9. Final Conclusion

This assignment provided practical experience with three important prompt-engineering techniques.

The zero-shot and few-shot experiments achieved identical classification accuracy of 100%. The mathematical reasoning experiment demonstrated that both direct and step-by-step prompts could solve the selected problem correctly, while step-by-step instructions produced a more organized response. The structured JSON experiment successfully extracted all required information across ten test cases.

The most important learning is that effective prompt engineering is not simply about writing longer instructions. It involves defining clear tasks, providing useful examples when needed, specifying output constraints, and evaluating the actual results.

These techniques can be applied in practical AI applications such as customer feedback analysis, automated invoice processing, document extraction, and intelligent assistants.

**Final takeaway:** Better prompts help make LLM behaviour more predictable, structured, and useful for real-world applications.

---

**End of Report**

**AI-2: Prompt Engineering**  
**Submitted by: Kammila Pavan Kumar**
