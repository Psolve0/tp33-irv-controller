# Contributing and adapting other panels

Project home: https://github.com/Psolve0/tp33-irv-controller
Questions and proposed changes: https://github.com/Psolve0/tp33-irv-controller/issues

Useful contributions include clearer installation instructions, accessibility improvements, documentation corrections and support for additional controller variants. The developer guide explains the existing protocol, code structure and adaptation points so both people and AI coding tools can build on the implementation.

When describing another panel, include its model/face variant, protocol differences and the required code changes. For a software issue, describe the app version, Android version, phone model, action attempted and behavior to reproduce. Images should omit personal reflections, surroundings, device addresses, serial numbers and location metadata. Review diagnostic exports before posting; never publish private signing material or personal contact information in an issue.

Before enabling a new controller mapping, establish what each command does and how it reports state. Start with passive communication and individual nonmovement controls. Keep unknown controls disabled, and keep physical controls accessible when implementing motion. A matching checksum or echoed command is not sufficient to establish correct output behavior. These are integration requirements, not a claim that similarly named panels are interchangeable.

Use a separate application ID/signing identity for an independently distributed fork. Consult the project's eventual license before redistributing source or derivatives; a source license has not yet been selected. This project has no built-in contribution enrollment, account service or automatic data submission.
