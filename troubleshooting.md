# Troubleshooting

## ALB is not reachable

- Confirm the ALB scheme is internet-facing.
- Confirm its state is Active.
- Confirm both public subnets are mapped.
- Confirm the ALB security group permits inbound HTTP on port 80.
- Review VPC routing and network ACLs.

## Target group reports unhealthy

- Confirm the instance is running.
- Check that User Data completed and Apache is active.
- Check that the health-check path returns a successful response.
- Ensure the instance security group permits traffic from the ALB security group.
- Review network ACLs and subnet routing.

## Traffic does not appear to match 80/20

- Confirm both target groups are present in the listener forwarding action.
- Confirm weights are 80 and 20.
- Confirm both targets are healthy.
- Test with a larger sample of requests.
- Consider browser connection reuse and target-group stickiness settings.

## Green is not served after cutover

- Confirm the listener action has Blue weight 0 and Green weight 100.
- Confirm `Green-Server` is healthy in `TG-Green`.
- Test the Green instance directly.
- Test again using the ALB DNS name.
