# Ahab
Kubernetes AI Brainstorming

Ahab API - AI for Kubernetes

- Machine learning based on observations about the cluster 
- May fit into an operator pattern - TBD
- Operates in three progressively-responsible modes at the discretion of the cluster operators (these can all be constrained by label/namespace, etc) *Advisor* (tell about notable things but not do) vs. *mitigation* (fix automatically) vs. *manager mode* (tell & fix, deploy when…  )
    - Advisor recommends changes to the running cluster state, can also identify what it “would do” if it was in mitigation or manager mode Data horizon: short i.e. week of training
    - Mitigation (incident) mode is taking corrective/proactive actions based on intelligence stream + history + what is normal - aberrations that are “normal” but outside of the expected state i.e. manual deploys/rollbacks, helm activity, deploys etc., re-establishes homeostasis : medium i.e. month or two
    - Manager mode can perform proactive scaling, delay deployment until conditions are better (as defined by thresholds), optimization, tries to make a better normal  : long term i.e. 6 mo -- also generates ongoing stats that can help determine accuracy of the models
- Integration with a bot : “How is today different than…” “What’s wrong with ____” using natural language processing
    - temperature of the cluster, i.e. Smoky the Bear forest fire risk model
    - human-readable
    - last failure, last deploy, changes generally so events can still be human-correlated
    - current status (can be defined)
- Understands organism boundaries (the area over which data can be gathered and acted on)
    - Types of resources
    - Actions against the cluster
        - user generated
        - end-user generated
        - automated
        - hostile
        - expected/unexpected
    - Architectural
        - region
        - phys nodes
        - constraints
        - failover path - warming up failure path when things seem unstable
            - warm up the app
            - start DB replication
        - “Google Power” model
- Node agents and central “brain” 
- Can expert state for apples-to-apples upgrades, duplicate clusters
- Integration with Helm
    - Understanding Helm’s release object
    - How to feed geometry back to Helm/Tiller
        - dry run mode reads chart and compares potential outcomes, Ahab can make recommendations
        - if I increase memory to X does that still work
        - fully model the interaction before you run it
            - if you do X … the impact on the cluster is Y
            - predictive failure analysis
            - the Ahab data set allows for complete virtual analysis

Goal - a method of constant prediction analysis that compares actual/predicted and adjusts the coinciding model. There will be an action stream and a prediction stream at all times. Eventually they should converge.
