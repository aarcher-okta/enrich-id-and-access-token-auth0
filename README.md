/**
* Handler that will be called during the execution of a PostLogin flow.
*
* @param {Event} event - Details about the user and the context in which they are logging in.
* @param {PostLoginAPI} api - Interface whose methods can be used to change the behaviour of
the login.
*/
exports.onExecutePostLogin = async (event, api) => {
const namespace = 'https://travel0.net/';
const {user} = event;
const {user_metadata = {}, app_metadata = {}} = event.user;
const subscriptions = {};
subscriptions.user = app_metadata.subscription || {};
subscriptions.company = app_metadata.companySubscription || {};


// ID Token claims
api.idToken.setCustomClaim(`${namespace}gender`, user_metadata.gender);
api.idToken.setCustomClaim(`${namespace}birthday`, user_metadata.birthday);
api.idToken.setCustomClaim(`${namespace}locale`, user_metadata.locale);
api.idToken.setCustomClaim(`${namespace}location`, user_metadata.location);
api.idToken.setCustomClaim(`${namespace}employment`, user_metadata.employment);
api.idToken.setCustomClaim(`${namespace}handles`, user_metadata.handles);
api.idToken.setCustomClaim(`${namespace}fav_style`, user_metadata.fav_style);
api.idToken.setCustomClaim(`${namespace}fav_type`, user_metadata.fav_type);
api.idToken.setCustomClaim(`${namespace}promotion_opt_in`, user_metadata.promotion_opt_in ||
false);
api.idToken.setCustomClaim(`${namespace}identities`, user.identities.map(
i => `${i.provider}|${i.user_id}`
));
// Access Token Claims
api.accessToken.setCustomClaim(`${namespace}email`, user.email || user_metadata.email);
api.accessToken.setCustomClaim(`${namespace}name`, user.name || user_metadata.name);
api.accessToken.setCustomClaim(`${namespace}subscriptions`, subscriptions);
};
/**


/**
* Handler that will be invoked when this action is resuming after an external redirect. If
your
* onExecutePostLogin function does not perform a redirect, this function can be safely
ignored.
*
* @param {Event} event - Details about the user and the context in which they are logging in.
* @param {PostLoginAPI} api - Interface whose methods can be used to change the behavior of
the login.
*/
// exports.onContinuePostLogin = async (event, api) => {
// };
