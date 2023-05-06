import PouchDB from "pouchdb";

export class PouchDBCache {
	_db = new PouchDB("offline-tiles");

	put(url, existingRevision, timestamp, format, blob) {
		this._db
			.put({
				_id: url,
				_rev: existingRevision,
				timestamp: timestamp,
			})
			.then(
				function (status) {
					return this._db.putAttachment(
						url,
						"tile",
						status.rev,
						blob,
						format
					);
				}.bind(this)
			);
	}

	get(url, done) {
		this._db.get(url, { revs_info: true }, done).then((error, data) => {
			this._db.getAttachment(data._id, "tile").then(function (blob) {
				done(undefined, {
					timestamp: data.timestamp,
					_revs_info: data._revs_info,
					image: blob,
				});
			});
		});
	}
}
