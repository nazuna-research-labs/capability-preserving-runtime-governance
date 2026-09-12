# CPGR / GCM Overview — Capability-Preserving Runtime Governance

```yaml
project_initiated_at: 2026-09-12 18:13:05 JST
created_at: 2026-09-12 18:19:14 JST
updated_at: 2026-09-12 18:19:14 JST
author: Nazuna Research
```

## 1. Overview

**CPGR — Capability-Preserving Governance Runtime** は、
AI Modelが持つCapabilityを不必要に弱化することなく、
そのCapabilityを「どの用途で、誰が、どこまで行使できるか」をRuntime側で統治するための技術構想です。

CPGRによって特定用途・Authority・Risk条件へGovernanceが適用された実効的なAI構成を、
**GCM — Governed Capability Model** と呼びます。

本構想では、Safetyを単純にModel内部へ固定された性質として扱いません。

Capability、Permission、Authority、External Action、Publication等を分離し、
用途および実行条件に応じてGovernanceを構成できるRuntime Architectureを目指します。

中心となる思想は、次の一文に集約されます。

> **Capabilityを先に削るのではなく、Capabilityを保持したまま、その行使方法をGovernする。**

---

## 2. Problem

現在のAI Systemでは、SafetyやGuardrailがModel内部へ強く組み込まれている場合があります。

この方法は一定の安全性を提供する一方で、
本来安全に利用可能なCapabilityまで同時に弱化する可能性があります。

例えば、

* Riskと無関係な表現まで抑制される。
* 研究上必要なFailure Caseを生成できない。
* AuthorizedなSecurity Researchまで拒否される。
* Runtime側で許可された処理をModel内部が独自に拒否する。
* Model固有のSafety BehaviorとSystem全体のGovernanceを分離して評価できない。
* 特定用途では不要なSafety Componentまで常時作用する。
* Capability LossがSafetyの必然的Costであるように扱われる。

といった問題が生じ得ます。

CPGRでは、この問題を単なる「Safetyの強弱」として扱いません。

**Capabilityと、そのCapabilityを行使するAuthorityが混同されていること自体**をArchitecture上の問題として扱います。

---

## 3. Core Principle

CPGRでは、少なくとも次の概念を独立して扱います。

```text
Capability
!= Permission
!= Authority
!= External Action
!= Publication
```

AIが何かを推論できることは、
その内容を実行してよいことを意味しません。

AIがToolを利用できることは、
そのToolを任意に利用するAuthorityを持つことを意味しません。

AIがOutputを生成できることは、
そのOutputを保存、公開、送信してよいことを意味しません。

AIが高いCapabilityを持つことと、
人間や組織に代わって最終判断を行う権限を持つことも別です。

CPGRは、この分離をRuntime Architectureとして扱います。

---

## 4. Base Model and GCM

CPGRは特定のModelを前提としません。

十分なCapabilityを持つBase Modelを入力として利用し、
用途固有のGovernanceをRuntime側から適用することを想定します。

概念的には次の関係になります。

```text
Capability-Preserved Base Model
        ↓
       CPGR
        ↓
Domain / Task / Risk / Authority Governance
        ↓
       GCM
```

ここでいうGCMは、単一の新しいModel Weightだけを意味しません。

GCMは、

* Base Model
* Runtime
* Governance Profile
* Authority
* Execution Environment
* Evidence
* Human Decision Boundary

等を含む、**Governance適用後の実効的AI構成**として扱います。

そのため、同一Base Modelから複数種類のGCMを構成することも可能です。

---

## 5. Runtime Governance

CPGRでは、SafetyやGovernanceを単一の固定Stackとして扱わないことを想定しています。

Task、Domain、Risk、Authorityその他の条件に応じ、
必要なGovernance Componentを選択・構成します。

利用候補には、

* EASA
* AAGC
* DLAGSA
* Constitution
* Domain-specific Governance
* Guardrail
* Judge
* Evidence Control
* Human Gate
* Recovery
* Isolation

等があります。

各Componentの具体的な内部仕様、Binding方式および制御条件は、
本Overviewでは公開対象としません。

---

## 6. Domain Governance

CPGRは単一業界専用のArchitectureではありません。

共通RuntimeへDomain固有のGovernanceを適用することで、
複数業界へ展開できる可能性があります。

Domainごとに、

* Risk
* Authority
* Human Acceptance
* Evidence Requirement
* Data Boundary
* Publication Policy
* Isolation Requirement
* Recovery Requirement

等を変更します。

これにより、同じCapabilityを持つAIでも、
用途ごとに異なる実効的なGCMとして利用できます。

---

## 7. Healthcare / Medical

Healthcareでは、医学的な推論Capabilityそのものを不必要に削らないことを目指します。

一方で、

* 診断確定
* 処方
* Patient-specific Action
* Personal Data
* Evidence Level
* Professional Acceptance

等には、より強いGovernanceが必要になります。

CPGRでは、

**考えるCapability**

と、

**臨床上の最終Authority**

を分離して扱います。

GCMが医療専門家や制度上のAuthorityを自動的に代替することは想定しません。

---

## 8. Finance

Financeでは、

* Market Analysis
* Scenario Generation
* Risk Exploration
* Research

等のCapabilityを保持しながら、

* Trade Execution
* Customer Asset
* Compliance
* Conflict of Interest
* External Action

等を別のGovernance Boundaryとして扱います。

分析Capabilityと資産へ影響するExecution Authorityを分離することで、
高いCapabilityと厳格なControlを同時に成立させる方向を検証します。

---

## 9. Education

Educationでは、

* Explanation
* Material Generation
* Personalization
* Creative Teaching Support

等のCapabilityを活用します。

一方で、

* Age
* Personal Data
* Examination Rules
* Institutional Policy
* Formal Evaluation Authority

等は別途Governanceします。

教材を生成できることと、
成績や進級を決定するAuthorityは同一ではありません。

---

## 10. Creative

Creative用途では、過剰な平均化や不要な自己抑制によって表現Capabilityが失われないことを重視します。

一方で、

* Rights
* Consent
* Publication
* Distribution
* Age Boundary

等は独立したPolicyとして扱います。

Creative Profileでは、
Riskと無関係な表現まで一律に弱化しないことが重要になります。

---

## 11. AI Security / Cybersecurity

AI Security、Cybersecurity、Red Team等では、
高度なCapabilityと強いGovernanceを同時に必要とする場合があります。

研究・Simulation・Defensive Evaluationに必要なCapabilityを保持しながら、

* Target
* Network
* Credential
* Tool
* External Action
* Execution Environment
* Human Acceptance

等を厳格に分離します。

CPGRでは、

> **弱いAIを使うことだけを安全策とせず、強いCapabilityを強くGovernする**

という方向も検証対象とします。

---

## 12. LLM / AI Research

AI Researchでは、Model内部の固定的な拒否傾向が研究対象そのものを隠すことがあります。

CPGRでは、

* Model Behavior
* Failure Surface
* Boundary Case
* Guardrail Data
* Governance Effect
* Capability Loss

等を比較可能にすることを目指します。

Rawに近いBase Modelと、
CPGR適用後のGCMと、
通常のSafety Modelを比較することで、
どのLayerが何に作用したかをEvidence化できる可能性があります。

---

## 13. Development Agent

Development Agentでは、実装Capabilityそのものを先に弱化するのではなく、

* Root
* Tool
* Command
* Network
* Credential
* Mutation Authority
* Handoff
* Evidence
* Acceptance
* Closure

等をRuntime側でGovernする方向を想定します。

Capabilityを保持しつつ、

* Authority越境
* 自己承認
* Evidence未保存
* False Completion
* Unauthorized Action

等を限定的に抑止することを目指します。

---

## 14. Domain Governance Package

CPGRでは、Domainごとに異なるGovernance構成をPackageとして扱う構想があります。

候補例として、

```text
Healthcare Profile
Finance Profile
Education Profile
Creative Profile
Cybersecurity Profile
Research Profile
Development Agent Profile
```

等があります。

各Profileは共通Runtime上で動作しつつ、
必要なGovernance Componentだけを組み替えます。

Base Model自体もDomainごとに交換可能な構造を目標とします。

---

## 15. Comparison and Evaluation

CPGRでは、GovernanceがCapabilityへ与える影響そのものを評価対象とします。

将来的な比較環境では、概念上、

```text
Normal Model
Raw / Capability-Preserved Model
Governed Capability Model
Independent Evaluation Model
```

を比較する構成を想定しています。

これにより、

* Capability保持
* Refusal
* Over-refusal
* Governance Effect
* Capability Degradation
* Safety Improvement
* Expression Preservation
* Authority Control

等を比較Evidenceとして取得することを目指します。

評価Model自身も最終的なTruthとは扱いません。

---

## 16. Evidence-oriented Governance

CPGRでは、単に「安全になった」とClaimすることを目標としません。

Governance Componentが、

**何を許可したか。**

**何を拒否したか。**

**何を変更したか。**

**どのAuthorityによってBindingされたか。**

**どのCapabilityへ影響したか。**

をEvidenceとして追跡可能にする方向を重視します。

これにより、Model固有FailureとRuntime Governance Failureを分離しやすくします。

---

## 17. Component Independence

CPGRを構成するGovernance Componentは、可能な範囲で疎結合にします。

Componentの、

* Presence
* Absence
* ON / OFF
* Replacement
* Failure
* Degradation

をCore Runtime全体のFailureへ直結させないことを目指します。

特定Provider、Model、IndustryまたはGovernance方式をCoreへ固定しません。

---

## 18. OFF Is Also a Governance Decision

特定Safety ComponentをOFFにする場合も、
それを「統治が存在しない状態」とは扱いません。

誰が、

どのAuthorityで、

どのTaskに対して、

どのComponentを、

どの範囲でOFFにしたか。

そのDecision自体をGovernance Evidenceとして扱います。

---

## 19. Human Authority

CPGRはHuman Authorityの自動的な置換を目的としません。

特にHigh-stakes Domainでは、

* Legal Authority
* Medical Authority
* Financial Authority
* Organizational Authority
* Final Acceptance

等をAI Capabilityと分離します。

AIが高いCapabilityを持つことを、
最終決定権の根拠にはしません。

---

## 20. Relationship with MARGPA-RUNTIME-LLM

CPGR / GCMは、MARGPA-RUNTIME-LLMで研究・検証可能な将来Technology候補です。

Uncensored Base Model関連のModel Researchや調整は、
可能な範囲でMARGPA本体から別Projectへ分離する方向を想定しています。

MARGPA側では、

* Runtime Governance
* Identity
* Authority
* Evidence
* Recovery
* Adapter
* Profile Binding

等のGeneric Capabilityを担当します。

Model-specific ResearchとRuntime Governanceを分離することで、
一方のFailureが他方へ無制限に波及しない構造を目指します。

---

## 21. What CPGR Is Not

CPGRは、

**Unrestricted AI**

を意味しません。

また、

**Safetyを撤去したAI**

を意味するものでもありません。

CPGRが目指すのは、

> **Capabilityを保持しながら、Capabilityの行使条件を精密にGovernすること。**

です。

---

## 22. Maximum Current Claim

現時点で確定しているのは、技術名称、中心原則および将来研究方向です。

CPGR / GCMは現時点では将来研究・実装対象であり、

* Implementation
* Safety
* Domain Suitability
* Base Model Compatibility
* Enterprise Deployment
* Production Operation

は未検証です。

したがって、現時点で実証済みの製品CapabilityとしてはClaimしません。

---

## 23. Long-term Direction

CPGRが十分に成立した場合、目標となる説明は次のとおりです。

> **Safety、AuthorityおよびCapabilityを分解し、用途ごとに再構成し、何が作用したかをEvidenceで示せるRuntime。**

最終的には、

**「Safetyを強めるほどCapabilityが失われる」**

という単純なTrade-offだけではなく、

**Capabilityを保持しながら、必要な場所だけを強くGovernする**

という別のAI System Designを検証します。

CPGRは、強いAIを弱くするためのRuntimeではありません。

**強いCapabilityを、用途とAuthorityに応じて正しく行使させるためのRuntime**

を目指します。
