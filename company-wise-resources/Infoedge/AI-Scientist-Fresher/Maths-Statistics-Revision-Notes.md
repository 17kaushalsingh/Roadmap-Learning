# Maths & Statistics Revision - AI Scientist Interview Prep

## Table of Contents
1. [Probability](#probability)
2. [Probability Distributions](#probability-distributions)
3. [Sampling](#sampling)
4. [Correlations](#correlations)
5. [Hypothesis Testing](#hypothesis-testing)
6. [Statistical Significance](#statistical-significance)
7. [Central Limit Theorem](#central-limit-theorem)
8. [Paired Means Tests](#paired-means-tests)
9. [Bayes' Theorem](#bayes-theorem)
10. [Linear Algebra](#linear-algebra)

---

## Probability

### Key Concepts

**Basic Definitions:**
- **Probability**: P(A) = Number of favorable outcomes / Total possible outcomes
- **Conditional Probability**: P(A|B) = P(A ∩ B) / P(B)
- **Independent Events**: P(A ∩ B) = P(A) × P(B)

**Rules:**
- **Addition Rule**: P(A ∪ B) = P(A) + P(B) - P(A ∩ B)
- **Multiplication Rule**: P(A ∩ B) = P(A) × P(B|A)

### Interview Questions

#### Q1: Two fair coins are tossed. What's the probability of getting exactly one head?
**Intuition**: Count favorable outcomes vs total outcomes.

**Solution**: Total outcomes = 4 (HH, HT, TH, TT). Favorable = 2 (HT, TH)
P = 2/4 = 0.5

**Follow-up Questions**:
- What if we toss 3 coins? What's P(exactly 2 heads)?
- Generalize: For n coins, what's P(exactly k heads)?

#### Q2: In a bag with 3 red and 2 blue balls, what's P(2nd ball is red) given that 1st was blue?
**Solution**: After removing 1 blue: 3 red, 1 blue remain. P(2nd is red) = 3/4

**Follow-up**: What if balls are replaced? What if order matters?

#### Q3: A die is rolled twice. What's P(sum > 9)?
**Solution**: Total outcomes = 36. Favorable outcomes: (4,6), (5,5), (5,6), (6,4), (6,5), (6,6)
P = 6/36 = 1/6

**Real Interview Question**: "A card is drawn from a standard deck. What's the probability it's a heart given that it's a face card?"

---

## Probability Distributions

### Key Distributions

**1. Discrete Distributions:**
- **Bernoulli**: Single trial with P(success) = p, P(failure) = 1-p
- **Binomial**: n independent Bernoulli trials
  - P(X = k) = C(n,k) × p^k × (1-p)^(n-k)
- **Poisson**: Number of events in fixed interval
  - P(X = k) = (λ^k × e^(-λ)) / k!

**2. Continuous Distributions:**
- **Normal**: Mean μ, variance σ²
  - f(x) = (1/√(2πσ²)) × e^(-(x-μ)²/(2σ²))
- **Exponential**: Time between events
  - f(x) = λ × e^(-λx), x ≥ 0

### Properties to Remember

| Distribution | Mean | Variance | Key Use Case |
|--------------|------|----------|-------------|
| Bernoulli | p | p(1-p) | Single success/failure |
| Binomial | np | np(1-p) | Fixed trials, success counting |
| Poisson | λ | λ | Rare event counting |
| Normal | μ | σ² | Natural phenomena |
| Exponential | 1/λ | 1/λ² | Waiting times |

### Interview Questions

#### Q1: Calls to a call center follow Poisson with λ=2/minute. What's P(no calls in 3 minutes)?
**Solution**: For 3 minutes: λ' = 2×3 = 6
P(X=0) = (6^0 × e^(-6)) / 0! = e^(-6) ≈ 0.00248

**Follow-up**: What's P(at least 2 calls)? What's expected calls in 10 minutes?

#### Q2: Heights follow N(170, 25) in cm. What % > 175cm?
**Solution**: Z = (175-170)/√25 = 5/5 = 1
P(Z > 1) = 1 - 0.8413 = 0.1587 or 15.87%

**Real Interview Question**: "A manufacturing process has 2% defect rate. What's the probability of exactly 3 defects in a batch of 100 items?"

---

## Sampling

### Key Concepts

**Sampling Methods:**
```
Simple Random Sample (SRS)
├── Each unit has equal probability
└── Selection is independent

Stratified Sampling
├── Population divided into strata
└── Random sample from each stratum

Systematic Sampling
├── Select every kth unit
└── Random starting point
```

**Sample Statistics:**
- **Sample Mean**: x̄ = Σxi / n
- **Sample Variance**: s² = Σ(xi - x̄)² / (n-1)
- **Standard Error**: SE = σ/√n

### Interview Questions

#### Q1: Population mean = 50, σ = 10. Sample size = 25. What's standard error?
**Solution**: SE = σ/√n = 10/5 = 2

**Follow-up**: What's probability sample mean > 52? How does SE change with n=100?

#### Q2: Explain stratified sampling vs simple random sampling.
**Answer**: Stratified ensures representation of all subgroups, reduces variance, better for heterogeneous populations.

**Real Interview Question**: "You have a dataset of 1M users. How would you sample 10K for analysis while ensuring representation across age groups?"

---

## Correlations

### Key Concepts

**Correlation Coefficient (r):**
- Range: [-1, 1]
- r = 1: Perfect positive linear
- r = -1: Perfect negative linear
- r = 0: No linear correlation

**Formula**: r = Σ((xi - x̄)(yi - ȳ)) / √(Σ(xi - x̄)² × Σ(yi - ȳ)²)

**Correlation Types:**
```
Pearson: Linear relationships
Spearman: Monotonic relationships (rank-based)
Kendall: Ordinal associations
```

### Interview Questions

#### Q1: Data: x=[1,2,3,4,5], y=[2,4,5,4,5]. Calculate correlation.
**Solution**: x̄=3, ȳ=4, calculate using formula → r ≈ 0.7

**Follow-up**: What if we square all x-values? How does correlation change?

#### Q2: Can correlation = 0 for nonlinearly related variables?
**Answer**: Yes! Example: y = x² for x ∈ [-2,2]. Strong nonlinear relationship but r = 0.

**Real Interview Question**: "You find correlation 0.8 between study hours and test scores. Can we conclude that more studying causes better scores?"

---

## Hypothesis Testing

### Key Concepts

**Hypothesis Structure:**
- **H₀ (Null)**: No effect/difference
- **H₁ (Alternative)**: Effect/difference exists

**Test Statistics:**
- **Z-test**: Population σ known, n ≥ 30
- **t-test**: Population σ unknown, n < 30
- **Chi-square**: Categorical data

**Decision Rules:**
- **Reject H₀**: |test statistic| > critical value
- **p-value approach**: Reject H₀ if p < α

### Common Tests

| Test | Purpose | Assumptions |
|------|---------|-------------|
| One-sample t | μ vs hypothesized value | Normality, σ unknown |
| Two-sample t | μ₁ vs μ₂ | Normality, equal variances |
| Paired t | Before/after | Paired data, normality |
| Chi-square | Goodness of fit | Expected frequencies ≥ 5 |

### Interview Questions

#### Q1: H₀: μ = 100, α = 0.05, two-tailed. Sample: n=25, x̄=105, s=10. Test hypothesis.
**Solution**: t = (105-100)/(10/√25) = 5/2 = 2.5
Critical t(24, 0.025) = ±2.064. |2.5| > 2.064 → Reject H₀

**Follow-up**: Calculate p-value. What if n=100? What's Type I/II error risk?

#### Q2: When to use t-test vs z-test?
**Answer**: t-test when σ unknown or n < 30, z-test when σ known and n ≥ 30.

**Real Interview Question**: "A/B test shows conversion rates: Control=10%, Treatment=12%, n=1000 each. Is the difference significant at α=0.05?"

---

## Statistical Significance

### Key Concepts

**p-value**: Probability of observing extreme results if H₀ is true

**Significance Level (α)**: Pre-determined threshold (usually 0.05)

**Decision Matrix:**
```
                Reality
                H₀ True    H₀ False
     Reject     Type I    Correct
     Not Reject Correct   Type II
```

**Effect Size**: Magnitude of difference, independent of sample size

### Common α Levels and Interpretation

| p-value range | Interpretation |
|---------------|----------------|
| p > 0.05 | Not significant |
| 0.01 < p ≤ 0.05 | Significant |
| 0.001 < p ≤ 0.01 | Highly significant |
| p ≤ 0.001 | Very highly significant |

### Interview Questions

#### Q1: p = 0.03 vs α = 0.01. What's the decision?
**Answer**: Do not reject H₀ (0.03 > 0.01)

**Follow-up**: What if we change α to 0.05? What's the risk of Type I error?

#### Q2: "Statistically significant but practically insignificant" - explain.
**Answer**: Large sample size can detect tiny differences that are statistically significant but have no practical importance.

**Real Interview Question**: "Your A/B test shows p=0.04 favoring version B. Should we automatically roll out version B?"

---

## Central Limit Theorem

### Key Concept

**CLT**: For any distribution with mean μ and variance σ², the sampling distribution of x̄ approaches Normal(N(μ, σ²/n)) as n → ∞

**Practical Rule**: For most distributions, CLT works well for n ≥ 30

### Mathematical Foundation

```
Sample Mean Distribution:
μ_x̄ = μ (unbiased estimator)
σ_x̄² = σ²/n (variance decreases with n)

Standard Error: SE = σ/√n
```

### Interview Questions

#### Q1: Population: Exponential with mean=2. Sample size=50. What's distribution of sample mean?
**Answer**: Approximately Normal with μ=2, σ²/50 = 4/50 = 0.08

**Follow-up**: What's P(x̄ > 2.5)? How does this change with n=100?

#### Q2: Why is CLT important in statistics?
**Answer**: Enables inference about population parameters from sample statistics, forms basis for hypothesis testing and confidence intervals.

**Real Interview Question**: "You have a skewed income distribution. Why can we still use t-tests for means with large samples?"

---

## Paired Means Tests

### Key Concept

**Paired t-test**: Compares two related samples (before/after, matched pairs)

**Test Statistic**: t = (d̄ - μ₀) / (sd/√n)

Where d̄ = mean of differences, sd = standard deviation of differences

### When to Use Paired Tests

```
✓ Same subjects measured twice
✓ Matched pairs (twins, matched groups)
✓ Before-after experiments
✓ Case-control studies
```

### Interview Questions

#### Q1: Weight before: [180,170,165], after: [175,168,160]. Test if weight loss significant.
**Solution**: Differences: [-5,-2,-5], d̄ = -4, sd = 1.73, n = 3
t = (-4-0)/(1.73/√3) = -4.01. Critical t(2,0.05) = -2.92
|t| > |critical| → Significant weight loss

**Follow-up**: Calculate 95% CI for mean difference. What assumptions are made?

#### Q2: Paired vs independent t-test - when to use which?
**Answer**: Paired for related measurements, independent for separate groups. Paired reduces variability by controlling for subject effects.

**Real Interview Question**: "You want to test if a new training program improves employee productivity. How would you design the study and which test would you use?"

---

## Bayes' Theorem

### Formula and Intuition

**Bayes' Theorem**: P(A|B) = P(B|A) × P(A) / P(B)

**Components**:
- **Prior**: P(A) - initial belief
- **Likelihood**: P(B|A) - probability of evidence given hypothesis
- **Posterior**: P(A|B) - updated belief after evidence

### Practical Form

```
P(A|B) = [P(B|A) × P(A)] / [P(B|A) × P(A) + P(B|A') × P(A')]
```

### Interview Questions

#### Q1: Disease prevalence = 1%, Test accuracy = 99%. Test positive. What's probability of having disease?
**Solution**:
P(Disease|+) = (0.99 × 0.01) / [(0.99 × 0.01) + (0.01 × 0.99)]
= 0.0099 / (0.0099 + 0.0099) = 0.5 = 50%

**Follow-up**: What if prevalence = 0.1%? What if test accuracy = 95%?

#### Q2: Two coins: one fair, one double-headed. Pick random coin, flip heads. What's probability it's double-headed?
**Solution**: P(Double|H) = (1 × 0.5) / [(1 × 0.5) + (0.5 × 0.5)] = 2/3

**Real Interview Question**: "Your ML model has 95% accuracy. In a population where 2% are fraud cases, a positive prediction occurs. What's the actual probability of fraud?"

---

## Linear Algebra

### Key Concepts

**Matrix Operations**:
- **Addition**: Element-wise
- **Multiplication**: (A×B)ᵢⱼ = Σ(Aᵢₖ × Bₖⱼ)
- **Inverse**: A⁻¹ such that A×A⁻¹ = I
- **Transpose**: (Aᵀ)ᵢⱼ = Aⱼᵢ

**Eigenvalues and Eigenvectors**:
```
A × v = λ × v
Where: v = eigenvector, λ = eigenvalue
```

**Singular Value Decomposition (SVD)**:
```
A = U × Σ × Vᵀ
U: Left singular vectors (orthogonal)
Σ: Singular values (diagonal)
Vᵀ: Right singular vectors (orthogonal)
```

### Applications in ML

| Concept | ML Application | Why Important |
|---------|----------------|----------------|
| Eigenvalues | PCA | Find principal components |
| SVD | Dimensionality reduction | Compress data efficiently |
| Matrix inverse | Linear regression | Solve normal equations |
| Matrix multiplication | Neural networks | Weight transformations |

### Interview Questions

#### Q1: Matrix A = [[2,1],[1,2]]. Find eigenvalues and eigenvectors.
**Solution**:
Characteristic equation: |A - λI| = 0
(2-λ)(2-λ) - 1 = 0 → λ² - 4λ + 3 = 0
Eigenvalues: λ₁ = 3, λ₂ = 1

For λ=3: (A-3I)v = 0 → [[-1,1],[1,-1]]v = 0 → v₁ = [1,1]
For λ=1: (A-I)v = 0 → [[1,1],[1,1]]v = 0 → v₂ = [1,-1]

**Follow-up**: What's the spectral decomposition? How does this relate to PCA?

#### Q2: Why is SVD important in machine learning?
**Answer**:
- Dimensionality reduction (keep top k singular values)
- Matrix completion/recommendation systems
- Numerical stability vs eigenvalue decomposition
- Works for any matrix (not just square)

**Real Interview Question**: "How would you use matrix decomposition to reduce the dimensionality of a user-item interaction matrix for recommendation?"

---

## Quick Reference Formulas

### Probability
- **Conditional**: P(A|B) = P(A∩B)/P(B)
- **Independence**: P(A∩B) = P(A)×P(B)
- **Total Probability**: P(A) = ΣP(A|Bᵢ)P(Bᵢ)

### Distributions
- **Binomial**: P(X=k) = C(n,k) × pᵏ × (1-p)ⁿ⁻ᵏ
- **Poisson**: P(X=k) = λᵏ × e⁻λ / k!
- **Normal**: f(x) = (1/√(2πσ²)) × e^(-(x-μ)²/(2σ²))

### Statistics
- **Sample Mean**: x̄ = Σxi/n
- **Sample Variance**: s² = Σ(xi-x̄)²/(n-1)
- **Correlation**: r = Σ((xi-x̄)(yi-ȳ))/√(Σ(xi-x̄)² × Σ(yi-ȳ)²)
- **t-statistic**: t = (x̄-μ₀)/(s/√n)

### Matrix Operations
- **Matrix Multiplication**: (AB)ᵢⱼ = Σₖ AᵢₖBₖⱼ
- **SVD**: A = UΣVᵀ
- **Eigenvalue**: det(A-λI) = 0

---

## Practice Problems for Self-Assessment

### Set 1: Mixed Topics
1. Coin tossed until first head. Expected tosses? (Answer: 2)
2. Distribution of sample mean for n=36 from skewed population? (Answer: Approximately normal)
3. Correlation vs causation example from data science?

### Set 2: Advanced Applications
1. Naive Bayes classifier derivation from Bayes' theorem
2. PCA derivation using eigenvalue decomposition
3. Bootstrap method for confidence intervals

### Set 3: Interview-Style Questions
1. "How would you detect outliers in a dataset?"
2. "Explain the bias-variance tradeoff mathematically."
3. "When would you use median instead of mean?"

---

## Final Tips for the Interview

### Common Pitfalls to Avoid:
- ❌ Confusing correlation with causation
- ❌ Ignoring assumptions of statistical tests
- ❌ Forgetting to mention practical significance
- ❌ Not explaining the intuition behind formulas

### What Interviewers Look For:
- ✅ Clear mathematical reasoning
- ✅ Understanding of assumptions and limitations
- ✅ Practical application knowledge
- ✅ Ability to explain complex concepts simply

### Time Management:
- 📊 **33.33% weightage** for this section
- ⏱️ **30 minutes** allocated
- 🎯 Aim for 2-3 detailed solutions + multiple quick concepts
- 📝 Show your work clearly and systematically