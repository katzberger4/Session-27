Gaia DR3 chromospheric activity sample: nearby stars with a Ca II
infrared triplet activity index

Built 2026-11-18 by make_gaia_sample.py (Day 4 directory of the lecture
materials), from a June 2022 pull of the Gaia archive.  The parent
sample is every source in gaiadr3.astrophysical_parameters with a
finite activityindex_espcs (the Ca II IRT activity index measured from
RVS spectra; Lanzafame et al. 2023, A&A 674, A30), joined to
gaiadr3.gaia_source and cut at parallax > 1 mas and
parallax_over_error > 5.  That is 1.9 million stars, nearly all
brighter than G = 13 because RVS spectra are needed.  It is a sample
of stars with good RVS spectra, not a volume-complete sample.

Files
  stars_200pc.csv.gz    297123 rows.  Every parent star within 200 pc
                        (35 MB; 140 MB in memory with pandas).
  active_500pc.csv.gz   17318 rows.  The active dwarfs within 500 pc:
                        logg_gspphot > 4 and log10_rprime_irt > -4.9
                        (2 MB).  Small enough to embed in a web page.
  Both have the same 46 columns.  The two files overlap.

  import pandas as pd
  df = pd.read_csv("stars_200pc.csv.gz")

Columns
  source_id                  Gaia DR3 source id (64-bit integer)
  ra, dec, l, b              deg, ICRS and Galactic, epoch 2016.0
  parallax, distance_pc      mas; distance_pc = 1000 / parallax
  parallax_over_error
  pmra, pmdec                mas/yr (pmra includes cos dec)
  radial_velocity, radial_velocity_error
                             km/s; missing for 2% of stars
  ruwe, non_single_star      astrometric fit quality; non_single_star
                             is the Gaia bit flag (0 = no orbit
                             solution)
  phot_g_mean_mag, phot_bp_mean_mag, phot_rp_mean_mag, bp_rp
                             apparent magnitudes, not dereddened
  M_G                        absolute G from the parallax, not
                             dereddened
  x_pc, y_pc, z_pc           Galactocentric position (astropy v4.0
                             frame).  The Sun is at x = -8122, y = 0,
                             z = 21.  x increases toward the Galactic
                             center.
  v_l, v_b                   tangential velocity along l and b, km/s,
                             from the proper motion and parallax
  teff_gspphot, logg_gspphot, mh_gspphot, ag_gspphot,
  ebpminrp_gspphot, radius_gspphot
                             GSP-Phot parameters from the BP/RP
                             spectra (K, cgs dex, dex, mag, mag,
                             Rsun); missing for 9% of stars
  vbroad, vbroad_error       RVS line broadening, km/s; missing for
                             47% of stars
  ew_espels_halpha, ew_espels_halpha_uncertainty
                             H-alpha pseudo-equivalent width from the
                             RP spectrum, nm.  Negative means emission.
  activityindex_espcs, activityindex_espcs_uncertainty
                             Ca II IRT activity index, nm: excess
                             equivalent width in the line cores over
                             an inactive template
  activityindex_espcs_input  M1 or M2: which set of stellar parameters
                             chose the template (GSP-Spec or GSP-Phot)
  log10_rprime_irt           log10 R'_IRT, the index converted to a
                             flux ratio with teff_gspphot (Lanzafame
                             et al. 2023).  Larger is more active.
                             Missing for 20% of stars (no
                             teff_gspphot, or outside the calibrated
                             range).
  cdips_cluster, cdips_log10_age, cdips_reference
                             cluster or association from the CDIPS
                             target list (Bouma et al. 2019, ApJS 245,
                             13), log10 of its age in years, and the
                             membership paper.  Comma-separated when
                             more than one paper lists the star.
                             Present for 6% of stars.  A missing value
                             means "not in the list", not "field star".
  vrm_period, vrm_period_error, vrm_activity_index_g,
  vrm_activity_index_g_error, vrm_n_segments
                             rotation period (days), photometric
                             amplitude index in G (mag), and number of
                             time-series segments from
                             gaiadr3.vari_rotation_modulation
                             (Distefano et al. 2023, A&A 674, A20).
                             Present for 2386 stars in stars_200pc and
                             868 in active_500pc.

Known answers
  Pleiades: 307 stars in stars_200pc have "Pleiades" or "Melotte_22"
  in cdips_cluster.  Median distance 136 pc, median proper motion
  (pmra, pmdec) = (+19.9, -45.3) mas/yr, cdips_log10_age 8.04.  A tool
  that links proper motion to the color-magnitude diagram must put
  that clump on one narrow main sequence.

  Activity and age: 25% of those Pleiades stars have
  log10_rprime_irt > -4.9.  For Praesepe (212 stars, log age 8.85) it
  is 2%, and for all dwarfs (logg_gspphot > 4) in the file it is 3%.

Caveats
  - source_id does not fit in a JavaScript number (it needs 64 bits;
    JavaScript keeps 53).  Pass it to a web page as a string.
  - cdips_cluster is a raw string.  The same cluster appears under
    several names ("Pleiades", "Melotte_22"; "Alpha_per", "Alpha per",
    "Melotte_20"), and some labels are loose associations with heavy
    field contamination ("Oh2017", "ScoOB2", "Zari2018ums").
  - The Gaia scanning law is imprinted on the sky density of this
    sample, because stars need enough RVS transits for an index.
  - In accreting pre-main-sequence stars the index measures
    accretion as well as chromospheric activity (Lanzafame et al.
    2023).
  - Distances are 1/parallax.  That is fine at parallax_over_error > 5
    for plotting, and no more.

References
  Lanzafame et al. 2023  https://scixplorer.org/abs/2023A&A...674A..30L/abstract
  Distefano et al. 2023  https://scixplorer.org/abs/2023A&A...674A..20D/abstract
  Bouma et al. 2019      https://scixplorer.org/abs/2019ApJS..245...13B/abstract
