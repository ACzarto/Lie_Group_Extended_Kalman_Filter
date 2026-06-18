# Lie_Group_Extended_Kalman_Filter
This project extends the traditional Euclidean Extended Kalman Filter (EKF) to the Lie group framework to address estimation problems involving states evolving on SO(3), resulting in the Lie Group Extended Kalman Filter (LG-EKF). 

We proposed this algorithm since, the main question proposed in the thesis is: In the absence of GNSS signals, is it still possible to solve the localization problem? A critical subtlety inherent to this framework is that, given the underlying physical geometry, certain quantities to be estimated reside in non-Euclidean spaces. Consequently, the canonical theory—originally developed under the assumption of vector spaces endowed with standard Euclidean metrics—necessitated a paradigm shift to properly accommodate the geometric peculiarities of these models. 

The discrete-time navigation model maps vehicle dynamics under a flat-Earth scenario, neglecting terrestrial rotation and Coriolis forces and we have in this formulation five hidden states to be estimated. We describe the structural domains using the cartesian product given by:


$$ [\mathbf{p}_n,  \mathbf{v}_n, \, \mathbf{R}_n,  \mathbf{a}_{b,n}, \mathbf{\omega}_{b,n}]  \in \mathbb{R}^{3} \times\mathbb{R}^{3} \times SO(3)\times \mathbb{R}^{3} \times\mathbb{R}^{3}$$

The position and velocity coordinates relate to an inertial coordinate frame aligned with the runway threshold. The raw IMU measurements ($a_{m,n}$) originate in the moving aircraft body frame and multiplying by $R_n$ converts the filtered specific force into the global reference system. Subtracting the bias ($a_{b,n}$) and process noise ($\epsilon_{a,n}$) isolates the kinematic acceleration. We add the gravity vector $\mathbf{g}$ to compensate for the accelerometer reaction force. Note that accelerometers do not measure the true kinematic translation acceleration in space; instead, they measure the so-called specific force, which is the difference between the body's true acceleration and the acceleration of gravity ($\mathbf{g}$).

The rotation matrix depends on the wedge operator $\wedge: \mathfrak{so}(3) \to SO(3)$ whose calculation is defined in other repository (we transform the measure from the Lie algebra to the Lie group).

For the sensor biases, we consider a first-order autoregressive processes, $\text{AR}(1)$, that model the bias instabilities. The memory parameters $\xi_a, \xi_\omega \in [0, 1]$ govern temporal correlation; setting these parameters to 1 reduces the models to classical random walks. Zero-mean white Gaussian noise sequences ($\epsilon_{b,n}^a, \epsilon_{b,n}^\omega$) drive the processes.

For solving this problem it is adopted as dynamical model the following system of equations

$$
\mathbf{p}_{n+1} = \mathbf{p}_n + \mathbf{v}_n \Delta T + \frac{1}{2}\left[\mathbf{R}_n(\mathbf{a}_{m,n} - \mathbf{a}_{b,n} - \mathbf{\epsilon}_{a,n}) + \mathbf{g}\right](\Delta T)^2
$$

$$
\mathbf{v}_{n+1} = \mathbf{v}_n + \left[\mathbf{R}_n(\mathbf{a}_{m,n} - \mathbf{a}_{b,n} - \mathbf{\epsilon}_{a,n}) + \mathbf{g}\right]\Delta T
$$

$$
\mathbf{R}_{n+1} = \mathbf{R}_n \exp\left(\left[(\mathbf{\omega}_{m,n} - \mathbf{\omega}_{b,n} - \mathbf{\epsilon}_{\omega,n})\Delta T\right]^\wedge \right)
$$

$$
\mathbf{a}_{b,n+1} = \xi_a \mathbf{a}_{b,n} + \mathbf{\epsilon}_{b,n}^{a}
$$

$$
\mathbf{\omega}_{b,n+1} = \xi_{\omega}\mathbf{\omega}_{b,n} + \mathbf{\epsilon}_{b,n}^{\omega}
$$

Concerning the observation model, we consider that an onboard camera tracks four landmarks ($r = 1, 2, 3, 4$), yielding four independent measurement vector expressions. Projecting the relative displacement vectors into the local aircraft body frame, we write

$$  \mathbf{y}_{n,r} = \mathbf{R}_n^T(\mathbf{p}^r-\mathbf{p}_n) + \epsilon_{n,y}^r$$

Finally, the flowchart of the entire processing of the algorithm is given by the following figure
