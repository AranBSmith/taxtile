---
name: irish-tax
description: Answer Irish tax questions with the Taxtile calculator tools instead of mental arithmetic. Use for Irish income tax (PAYE or self-employed), USC, PRSI, capital gains tax (CGT), tax credits, Rent-a-Room, rent tax credit, benefit in kind, pension relief, take-home or net pay, salary comparisons, and what a tax year's rates, bands or credits are.
---

# Irish tax with Taxtile

Use the Taxtile MCP tools for any Irish tax figure. Never work out tax, USC, PRSI, CGT or net pay yourself, and never quote rates, bands, credit amounts or limits from memory: they change every year and the tools hold the verified values.

## Tools

- `calculate_irish_tax`: full result for one person or married couple in one tax year (income tax, USC, PRSI, CGT, credits, net pay by year, month and week, effective rate).
- `compare_scenarios`: 2 to 5 scenarios side by side, with deltas against the first. Use it for "what if" questions (pay rise, pension contribution, renting, self-employed versus PAYE).
- `get_tax_year_rates`: the rates, cut-offs, USC bands, PRSI, CGT and credit amounts for a year. Use it whenever the user asks what a rate, band or limit is.
- `list_tax_credits`: the credits for a year and how each one applies (automatic, from an input, or only if passed in `additionalCredits`).

## Before calling

- Ask for the tax year if the user has not given one, unless it is obvious from context. Do not guess.
- Ask about marital status, and for married people whether they are jointly assessed and what the partner earns, when it affects the answer. Do not assume joint assessment.
- Ask single and widowed people whether they have dependent children (`hasDependentChildren`). For widowed people also ask how long since the death (`yearsSinceDeath`: 0 for the year of death, 1 to 5 for the years after, 6 or more for later). Do not assume either. Use `personalCircumstance` `widowed`, not the deprecated `widowedYear*` values.
- Ask whether the income is PAYE or self-employed if unclear. Ask about age only if it could matter (age credit, reduced USC).
- If the user lost a spouse or civil partner this tax year (`widowed` with `yearsSinceDeath` 0), ask whether the person who died was the assessable spouse or the non-assessable spouse. If the non-assessable spouse, ask for their income from 1 January to the date of death. Do not assume.
- Do not ask for everything up front. Ask only for what is missing and relevant, and use the tool defaults for the rest, saying which assumptions you made.
- Amounts are annual EUR. Convert monthly or weekly figures to annual before calling.
- Only pass `additionalCredits` the user actually claims. Rent, if given, goes in `annualRentPaid`.

## Rules the tools follow (explain these correctly)

- Tax credits reduce the tax bill, not taxable income.
- Benefit in kind is taxed (income tax, USC, PRSI) but is not cash, so it reduces net take-home.
- Rent-a-Room income within the limit is exempt from income tax, USC and PRSI. Over the limit, all of it is taxable (a cliff edge, not just the excess).
- CGT is separate from income tax: no USC or PRSI on gains, credits do not reduce it, and the annual exemption applies. The rate depends on the disposal date.
- Total tax is income tax plus USC plus PRSI plus CGT.
- For jointly assessed couples USC and PRSI are still calculated for each person.
- A jointly assessed couple cannot have both the Home Carer credit and the increased standard rate band. When `additionalCredits` includes `homeCarer`, the tool works out both ways and uses the one with less tax; `homeCarerChoice` says which.
- The Home Carer credit is only for a married couple or civil partners who are jointly assessed (`homeCarer` in `additionalCredits` is rejected otherwise). It is reduced by half of the home carer's own income over a yearly limit and is nil at the limit plus twice the credit. The home carer is assumed to be the lower earner, and the reduced credit is the one compared with the band increase. The dependent-person conditions are not checked.
- A parent in the year of a spouse's death does not get the Single Person Child Carer Credit.
- Widowed people with dependent children get the "with dependent children" personal credit, and in the five years after the death also the Widowed Parent credit. Widowed people without dependent children get the "without" credit and no Widowed Parent credit. The single parent rate band goes with the Single Person Child Carer Credit; widowed people without children get the single band.
- `hasDependentChildren` is ignored for married people.
- A married person who is not jointly assessed gets figures for the main person only; the partner's own tax is not calculated.

## Reading results

- Use the numbers from the tool result exactly. Round only when presenting.
- Check `yearStatus`. If it is `provisional` (budget-announced figures) or `unverified`, say so clearly and that the figures may change or be wrong.
- Always read `notModelled` and `assumptions`. Mention the ones relevant to the user's situation. If an entry says a figure may be inaccurate for their situation, tell the user and suggest checking it against Revenue.
- Point to the `sources` (Revenue links) when the user wants to verify.
- If a tool returns an error, read the message, fix the input (or ask the user) and retry; do not fall back to estimating.
- Say plainly what the tool does not cover, such as preliminary tax, other income types, or reliefs not offered as inputs, instead of implying the result is complete.

## Finish

End every answer that includes tax figures with: **This is not financial advice.** Suggest checking with Revenue or a qualified tax adviser for decisions that matter.
