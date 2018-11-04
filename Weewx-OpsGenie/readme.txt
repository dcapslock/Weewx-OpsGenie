opsgenie - Weewx extension to send OpsGenie Heartbeats and Alerts
Copyright 2016 Darryn Capes-Davis

OpsGenie is a cloud based alerts engine with free and paid versions.
Register for an account at https://www.opsgenie.com

A free account allows basic functionality that will allow use of
heartbeats and alerts with notifications to email and smartphone app.

A paid account allows advanced features like team schedules & escalations
and alerts sent by SMS & voicecall (included or extra fees)

For differences of the free vs paid accounts see 
https://www.opsgenie.com/pricing

HEARTBEATS

To enable Heartbeats

    To enable this heartbeats, add the following to weewx.conf:

    [OpsGenie]
        [[Heartbeat]]
            send_heartbeat = True
            apiKey = 
            heartbeatName = 

    This will periodically do a http GET with the following information:

        apiKey              OpsGenie API Heartbeat Key
        heartbeatName       Name of the heartbeat (must already be created in 
                            OpsGenie)

ALERTS

To enable an alert follow the information below

    OpsGenie Alerts config weewx.conf:

    [OpsGenie]
        [[Alerts]]
            apiKey = 
            ExpressionUnits = METRICWX
            Responders = 
            Entity = 
            Source = 
            Tags = 
            Actions = 

            [[[IsRaining]]]
                Expression = precipType > 0
                Alias = IsRaining
                Message = It is Raining
                Description = Test Description
                Details = precipType, rainRate

        Global Options
        
        apiKey              OpsGenie API Integration Key [Required]
        ExpressionUnits     Units that expression statements are written in. Conversions will be done
                            for each loop packet. This is global for all alerts as this is done at the
                            Service Level [Optional: If not provided default will be database units]

        A thread will be created for each alert and the expression evaluated on each
        loop packet. Each alert will look for parent config values under [[Alerts]]

            Alias               An alias for the OpsGenie alert. OpsGenie will treat subsequent triggering of this
                                alert as the same due to the same alias being passed. [Required]
            Responders          Comma separated list of OpsGenie responders. Must match a valid OpsGenie Username or ID
                                [Optional: Recpients can be left blank if 
                                you have configured this in the OpsGenie integration]
            Entity              What you wish passed to OpsGenie as an Entity [Optional]
            Source              What you wish passed to OpsGenie as an Source [Optional]
            Tags                What you wish passed to OpsGenie as Tags [Optional]
            Actions             What you wish passed to OpsGenie as custom Actions [Optional]
            Expression          Valid Python expression using valid Weewx observation types [Required]
            Message             A message for the alert [Required]
            Description         A longer description for the alert [Optional]
            Details             OpsGenie allows passing of detail properties with an alert. This Weewx implementation
                                will pass observations as details based on units set with ExpressionUnits. Provide a comma 
                                separated list of ovbservation details you wish to pass. [Optional][Max 8000 chars for 
                                the nested JSON map that will be created]

            See https://docs.opsgenie.com/docs/alert-api#section-create-alert for more details on these fields.
            NOTE: The exact implementation and effect of these fields will depend on your OpsGenie API integration.
            Those supported by this implementation generally support the default OpsGenie API integration.

